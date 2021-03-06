---
title: "Creare un gateway applicazione per l&quot;hosting di più siti | Microsoft Docs"
description: "Questa pagina contiene istruzioni per creare e configurare un gateway applicazione di Azure per l&quot;hosting di più applicazioni Web nello stesso gateway."
documentationcenter: na
services: application-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: b107d647-c9be-499f-8b55-809c4310c783
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/12/2016
ms.author: amsriva
translationtype: Human Translation
ms.sourcegitcommit: 8c4e33a63f39d22c336efd9d77def098bd4fa0df
ms.openlocfilehash: d42efa7d359f5c87c14afbfd138328b37c8ae6c2
ms.lasthandoff: 04/20/2017


---
# <a name="create-an-application-gateway-for-hosting-multiple-web-applications"></a>Creare un gateway applicazione per l'hosting di più applicazioni Web

> [!div class="op_single_selector"]
> * [Portale di Azure](application-gateway-create-multisite-portal.md)
> * [PowerShell per Azure Resource Manager](application-gateway-create-multisite-azureresourcemanager-powershell.md)

L'hosting di più siti consente di distribuire più applicazioni Web nello stesso gateway applicazione. La presenza dell'intestazione host nella richiesta HTTP in ingresso consente di determinare il listener che riceverà il traffico. Il listener indirizza quindi il traffico al pool back-end appropriato in base alla configurazione della definizione delle regole del gateway. Nelle applicazioni Web abilitate per SSL il gateway applicazione sceglie il listener corretto per il traffico Web in base all'estensione dell'indicazione nome server (SNI). L'hosting di più siti viene comunemente usato per bilanciare il carico delle richieste per diversi domini Web tra vari pool di server back-end. Analogamente, lo stesso gateway applicazione potrebbe ospitare anche più sottodomini dello stesso dominio radice.

## <a name="scenario"></a>Scenario

Nell'esempio seguente, il gateway applicazione gestisce il traffico per contoso.com e fabrikam.com con due pool di server back-end: il pool di server contoso e il pool di server fabrikam. Una configurazione simile potrebbe essere usata per ospitare sottodomini come app.contoso.com e blog.contoso.com.

![imageURLroute](./media/application-gateway-create-multisite-azureresourcemanager-powershell/multisite.png)

## <a name="before-you-begin"></a>Prima di iniziare

1. Installare la versione più recente dei cmdlet di Azure PowerShell usando l'Installazione guidata piattaforma Web. È possibile scaricare e installare la versione più recente dalla sezione **Windows PowerShell** della [Pagina di download](https://azure.microsoft.com/downloads/).
2. È necessario che i server aggiunti al pool back-end per l'uso del gateway applicazione esistano oppure che i relativi endpoint siano stati creati nella rete virtuale in una subnet separata o che sia stato assegnato loro un indirizzo IP/VIP pubblico.

## <a name="requirements"></a>Requisiti

* **Pool di server back-end:** elenco di indirizzi IP dei server back-end. Gli indirizzi IP elencati devono appartenere alla subnet della rete virtuale o devono essere indirizzi IP/VIP pubblici. È possibile usare anche FQDN.
* **Impostazioni del pool di server back-end:** ogni pool ha impostazioni quali porta, protocollo e affinità basata sui cookie. Queste impostazioni sono associate a un pool e vengono applicate a tutti i server nel pool.
* **Porta front-end:** porta pubblica aperta sul gateway applicazione. Il traffico raggiunge questa porta e quindi viene reindirizzato a uno dei server back-end.
* **Listener** : ha una porta front-end, un protocollo (Http o Https, con distinzione tra maiuscole e minuscole) e il nome del certificato SSL (se si configura l'offload SSL). Per i gateway applicazione abilitati per più siti vengono aggiunti anche indicatori SNI e nome host.
* **Regola**: associa il listener e il pool di server back-end e definisce il pool di server back-end a cui deve essere indirizzato il traffico quando raggiunge un listener specifico. Le regole vengono elaborate nell'ordine in cui sono elencate e il traffico verrà indirizzato tramite la prima regola corrispondente indipendentemente dalla specificità. Se ad esempio si dispone di due regole, una che usa un listener di base e una che usa un listener multisito, entrambe sulla stessa porta, la regola con il listener multisito deve essere elencata prima della regola con il listener di base per funzionare come previsto.

## <a name="create-an-application-gateway"></a>Creare un gateway applicazione

Per creare un gateway applicazione, seguire questa procedura:

1. Creare un gruppo di risorse per Gestione risorse.
2. Creare una rete virtuale, le subnet e un indirizzo IP pubblico per il gateway applicazione.
3. Creare un oggetto di configurazione del gateway applicazione.
4. Creare una risorsa del gateway applicazione.

## <a name="create-a-resource-group-for-resource-manager"></a>Creare un gruppo di risorse per Gestione risorse

Assicurarsi di usare la versione più recente di Azure PowerShell. Altre informazioni sono disponibili in [Uso di Azure PowerShell con Azure Resource Manager](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Passaggio 1

Accedere ad Azure

```powershell
Login-AzureRmAccount
```
Verrà richiesto di eseguire l'autenticazione con le proprie credenziali.

### <a name="step-2"></a>Passaggio 2

Controllare le sottoscrizioni per l'account.

```powershell
Get-AzureRmSubscription
```

### <a name="step-3"></a>Passaggio 3

Scegliere le sottoscrizioni ad Azure da usare.

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="step-4"></a>Passaggio 4

Creare un gruppo di risorse. Ignorare questo passaggio se si usa un gruppo di risorse esistente.

```powershell
New-AzureRmResourceGroup -Name appgw-RG -location "West US"
```

In alternativa, è anche possibile creare tag del gruppo di risorse per il gateway applicazione:

```powershell
$resourceGroup = New-AzureRmResourceGroup -Name appgw-RG -Location "West US" -Tags @{Name = "testtag"; Value = "Application Gateway multiple site"}
```

Gestione risorse di Azure richiede che tutti i gruppi di risorse specifichino un percorso che viene usato come percorso predefinito per le risorse presenti in tale gruppo di risorse. Assicurarsi che tutti i comandi per creare un gateway applicazione usino lo stesso gruppo di risorse.

Nell'esempio precedente è stato creato un gruppo di risorse denominato **appgw-RG** con la località **Stati Uniti occidentali**.

> [!NOTE]
> Se è necessario configurare un probe personalizzato per il gateway applicazione, vedere [Creare un probe personalizzato per il gateway applicazione di Azure con PowerShell per Azure Resource Manager](application-gateway-create-probe-ps.md). Per altre informazioni, vedere l'articolo relativo a [probe personalizzati e monitoraggio dell'integrità](application-gateway-probe-overview.md) .

## <a name="create-a-virtual-network-and-subnets"></a>Creare una rete virtuale e le subnet

L'esempio seguente illustra come creare una rete virtuale usando Gestione risorse. In questo passaggio vengono create due subnet. La prima subnet è per il gateway applicazione stesso. Il gateway applicazione richiede una propria subnet che deve contenere le sue istanze. In tale subnet possono essere distribuiti solo altri gateway applicazione. La seconda subnet viene usata per contenere i server back-end dell'applicazione.

### <a name="step-1"></a>Passaggio 1

Assegnare l'intervallo di indirizzi 10.0.0.0/24 alla variabile subnet da usare per contenere il gateway applicazione.

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -AddressPrefix 10.0.0.0/24
```
### <a name="step-2"></a>Passaggio 2

Assegnare l'intervallo di indirizzi 10.0.1.0/24 alla variabile subnet2 da usare per i pool back-end.

```powershell
$subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -AddressPrefix 10.0.1.0/24
```

### <a name="step-3"></a>Passaggio 3

Creare una rete virtuale denominata **appgwvnet** nel gruppo di risorse **appgw-rg** per l'area Stati Uniti occidentali usando il prefisso 10.0.0.0/16 con le subnet 10.0.0.0/24 e 10.0.1.0/24.

```powershell
$vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet,$subnet2
```

### <a name="step-4"></a>Passaggio 4

Assegnare una variabile di subnet per la creazione di un gateway applicazione nei passaggi successivi.

```powershell
$appgatewaysubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name appgatewaysubnet -VirtualNetwork $vnet
$backendsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name backendsubnet -VirtualNetwork $vnet
```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Creare un indirizzo IP pubblico per la configurazione front-end

Creare una risorsa IP pubblica denominata **publicIP01** nel gruppo di risorse **appgw-rg** per l'area Stati Uniti occidentali.

```powershell
$publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -name publicIP01 -location "West US" -AllocationMethod Dynamic
```

All'avvio del servizio viene assegnato un indirizzo IP al gateway applicazione.

## <a name="create-application-gateway-configuration"></a>Creare la configurazione del gateway applicazione

È necessario impostare tutti gli elementi di configurazione prima di creare il gateway applicazione. La procedura seguente consente di creare gli elementi di configurazione necessari per una risorsa del gateway applicazione.

### <a name="step-1"></a>Passaggio 1

Creare una configurazione IP del gateway applicazione denominata **gatewayIP01**. All'avvio, il gateway applicazione seleziona un indirizzo IP dalla subnet configurata e instrada il traffico di rete agli indirizzi IP nel pool di indirizzi IP back-end. Tenere presente che ogni istanza ha un indirizzo IP.

```powershell
$gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $appgatewaysubnet
```

### <a name="step-2"></a>Passaggio 2

Configurare i pool di indirizzi IP back-end denominati **pool01** e **pool2** con gli indirizzi IP **134.170.185.46**, **134.170.188.221**, **134.170.185.50** per **pool1** e **134.170.186.46**, **134.170.189.221**, **134.170.186.50** per **pool2**.

```powershell
$pool1 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 10.0.1.100, 10.0.1.101, 10.0.1.102
$pool2 = New-AzureRmApplicationGatewayBackendAddressPool -Name pool02 -BackendIPAddresses 10.0.1.103, 10.0.1.104, 10.0.1.105
```

Questo esempio mostra due pool back-end che indirizzano il traffico di rete in base al sito richiesto. Un pool riceve il traffico dal sito "contoso.com" e l'altro riceve il traffico dal sito "fabrikam.com". È necessario sostituire gli indirizzi IP precedenti e aggiungere gli endpoint di indirizzi IP dell'applicazione. Al posto di indirizzi IP interni si potrebbero usare per le istanze back-end anche indirizzi IP pubblici, FQDN o la scheda di interfaccia di rete di una VM. Per specificare FQDN invece di indirizzi IP in PowerShell usare il parametro "-BackendFQDNs".

### <a name="step-3"></a>Passaggio 3

Configurare le impostazioni **poolsetting01** e **poolsetting02** del gateway applicazione per il traffico di rete con carico bilanciato nel pool back-end. In questo esempio vengono configurate diverse impostazioni per i pool back-end. Ogni pool back-end può avere un'impostazione del pool back-end dedicata.

```powershell
$poolSetting01 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting01" -Port 80 -Protocol Http -CookieBasedAffinity Disabled -RequestTimeout 120
$poolSetting02 = New-AzureRmApplicationGatewayBackendHttpSettings -Name "besetting02" -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 240
```

### <a name="step-4"></a>Passaggio 4

Configurare l'indirizzo IP front-end con l'endpoint di indirizzo IP pubblico.

```powershell
$fipconfig01 = New-AzureRmApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-5"></a>Passaggio 5

Configurare la porta front-end per un gateway applicazione.

```powershell
$fp01 = New-AzureRmApplicationGatewayFrontendPort -Name "fep01" -Port 443
```

### <a name="step-6"></a>Passaggio 6

Configurare due certificati SSL per i due siti Web che verranno supportati in questo esempio: un certificato per il traffico di contoso.com e l'altro per il traffico di fabrikam.com. Questi certificati per i siti Web devono essere rilasciati da un'autorità di certificazione. I certificati autofirmati sono supportati, ma non sono consigliati per il traffico di produzione.

```powershell
$cert01 = New-AzureRmApplicationGatewaySslCertificate -Name contosocert -CertificateFile <file path> -Password <password>
$cert02 = New-AzureRmApplicationGatewaySslCertificate -Name fabrikamcert -CertificateFile <file path> -Password <password>
```

### <a name="step-7"></a>Passaggio 7

Configurare due listener per i due siti Web dell'esempio. Questo passaggio configura i listener per l'indirizzo IP pubblico, la porta e l'host usati per ricevere il traffico in ingresso. Il parametro HostName è necessario per il supporto di più siti e deve essere impostato sul sito Web appropriato per cui viene ricevuto il traffico. Il parametro RequireServerNameIndication dovrà essere impostato su true per i siti Web in cui è necessario il supporto per SSL per uno scenario di hosting multiplo. Se è necessario il supporto per SSL, si deve specificare anche il certificato SSL che viene usato per proteggere il traffico per l'applicazione Web. La combinazione di FrontendIPConfiguration, FrontendPort e HostName deve essere univoca per un listener. Ogni listener può supportare un certificato.

```powershell
$listener01 = New-AzureRmApplicationGatewayHttpListener -Name "listener01" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "contoso11.com" -RequireServerNameIndication true  -SslCertificate $cert01
$listener02 = New-AzureRmApplicationGatewayHttpListener -Name "listener02" -Protocol Https -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -HostName "fabrikam11.com" -RequireServerNameIndication true -SslCertificate $cert02
```

### <a name="step-8"></a>Passaggio 8

Creare l'impostazione di due regole per le due applicazioni Web di questo esempio. Una regola collega listener, pool back-end e impostazioni HTTP. Questo passaggio configura il gateway applicazione per l'uso di una regola di routing Basic per ogni sito Web. Il traffico verso ogni sito Web viene ricevuto dal rispettivo listener configurato e quindi indirizzato al pool back-end configurato usando le proprietà specificate in BackendHttpSettings.

```powershell
$rule01 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule01" -RuleType Basic -HttpListener $listener01 -BackendHttpSettings $poolSetting01 -BackendAddressPool $pool1
$rule02 = New-AzureRmApplicationGatewayRequestRoutingRule -Name "rule02" -RuleType Basic -HttpListener $listener02 -BackendHttpSettings $poolSetting02 -BackendAddressPool $pool2
```

### <a name="step-9"></a>Passaggio 9:

Configurare il numero di istanze e le dimensioni per il gateway applicazione.

```powershell
$sku = New-AzureRmApplicationGatewaySku -Name "Standard_Medium" -Tier Standard -Capacity 2
```

## <a name="create-application-gateway"></a>Creare il gateway applicazione

Creare un gateway applicazione con tutti gli oggetti di configurazione illustrati nei passaggi precedenti.

```powershell
$appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-RG -Location "West US" -BackendAddressPools $pool1,$pool2 -BackendHttpSettingsCollection $poolSetting01, $poolSetting02 -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener01, $listener02 -RequestRoutingRules $rule01, $rule02 -Sku $sku -SslCertificates $cert01, $cert02
```

> [!IMPORTANT]
> Il provisioning del gateway applicazione è un'operazione con esecuzione prolungata il cui completamento può richiedere tempo.
> 
> 

## <a name="get-application-gateway-dns-name"></a>Ottenere il nome DNS del gateway applicazione

Dopo avere creato il gateway, il passaggio successivo prevede la configurazione del front-end per la comunicazione. Quando si usa un IP pubblico, il gateway applicazione richiede un nome DNS assegnato in modo dinamico, non descrittivo. Per assicurarsi che gli utenti finali possano raggiungere il gateway applicazione, è possibile usare un record CNAME per fare riferimento all'endpoint pubblico del gateway applicazione. [Configurazione di un nome di dominio personalizzato in Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). A questo scopo, recuperare i dettagli del gateway applicazione e il nome DNS e l'IP associati, usando l'elemento PublicIPAddress collegato al gateway applicazione. Il nome DNS del gateway applicazione dovrà essere usato per creare un record CNAME che associa le due applicazioni Web a questo nome DNS. Non è consigliabile usare record A perché l'indirizzo VIP può cambiare al riavvio del gateway applicazione.

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
```

```
Name                     : publicIP01
ResourceGroupName        : appgw-RG
Location                 : westus
Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
ResourceGuid             : 00000000-0000-0000-0000-000000000000
ProvisioningState        : Succeeded
Tags                     : 
PublicIpAllocationMethod : Dynamic
IpAddress                : xx.xx.xxx.xx
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                                "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                            Configurations/frontend1"
                            }
DnsSettings              : {
                                "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                            }
```

## <a name="next-steps"></a>Passaggi successivi

Informazioni su come proteggere i siti Web con [Gateway applicazione: firewall applicazione Web](application-gateway-webapplicationfirewall-overview.md)


