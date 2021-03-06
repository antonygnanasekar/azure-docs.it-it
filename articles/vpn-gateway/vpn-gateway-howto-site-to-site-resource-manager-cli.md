---
title: 'Connettere la rete locale a una rete virtuale di Azure: VPN da sito a sito: interfaccia della riga di comando | Microsoft Docs'
description: "Passaggi per creare una connessione IPsec dalla rete locale a una rete virtuale di Azure tramite Internet pubblico. Questa procedura consentirà di creare una connessione gateway VPN da sito a sito cross-premise usando l&quot;interfaccia della riga di comando."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/21/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: c3563f3a3fa46d40ba02fe97b3b0ebe3c45caddd
ms.lasthandoff: 04/26/2017


---
# <a name="create-a-virtual-network-with-a-site-to-site-vpn-connection-using-cli"></a>Creare una rete virtuale con una connessione VPN da sito a sito usando l'interfaccia della riga di comando

Questo articolo illustra come usare l'interfaccia della riga di comando di Azure per creare una connessione gateway VPN da sito a sito dalla rete locale alla rete virtuale. I passaggi di questo articolo sono applicabili al modello di distribuzione Resource Manager. È anche possibile creare questa configurazione usando strumenti o modelli di distribuzione diversi selezionando un'opzione differente nell'elenco seguente:

> [!div class="op_single_selector"]
> * [Resource Manager - Portale di Azure](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Resource Manager - Interfaccia della riga di comando](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Distribuzione classica - Portale di Azure](vpn-gateway-howto-site-to-site-classic-portal.md)
> * [Distribuzione classica - Portale classico](vpn-gateway-site-to-site-create.md)
> 
>


![Diagramma della connessione cross-premise gateway VPN da sito a sito](./media/vpn-gateway-howto-site-to-site-resource-manager-cli/site-to-site-connection-diagram.png)

Una connessione gateway VPN da sito a sito viene usata per connettere la rete locale a una rete virtuale di Azure tramite un tunnel VPN IPsec/IKE (IKEv1 o IKEv2). Questo tipo di connessione richiede un dispositivo VPN che si trova in locale con un indirizzo IP pubblico esterno assegnato. Per altre informazioni sui gateway VPN, vedere [Informazioni sul gateway VPN](vpn-gateway-about-vpngateways.md).

## <a name="before-you-begin"></a>Prima di iniziare

Prima di iniziare la configurazione, verificare di soddisfare i criteri seguenti:

* Assicurarsi di voler usare il modello di distribuzione Resource Manager. [!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 
* Un dispositivo VPN compatibile e un utente che sia in grado di configurarlo. Per altre informazioni sui dispositivi VPN compatibili e sulla configurazione dei dispositivi, vedere [Informazioni sui dispositivi VPN](vpn-gateway-about-vpn-devices.md).
* Un indirizzo IPv4 pubblico esterno per il dispositivo VPN. L'indirizzo IP non può trovarsi dietro un NAT.
* Se non si ha familiarità con gli intervalli degli indirizzi IP disponibili nella configurazione della rete locale, è necessario coordinarsi con qualcuno che possa fornire tali dettagli. Quando si crea questa configurazione, è necessario specificare i prefissi degli intervalli di indirizzi IP che Azure instraderà alla posizione locale. Nessuna delle subnet della rete locale può sovrapporsi alle subnet della rete virtuale a cui ci si vuole connettere. 
* La versione più recente dei comandi dell'interfaccia della riga di comando (2.0 o successiva). Per informazioni sull'installazione dei comandi dell'interfaccia della riga di comando, vedere [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (Installare l'interfaccia della riga di comando di Azure 2.0).

### <a name="example-values"></a>Valori di esempio

È possibile usare i valori seguenti per creare un ambiente di test o fare riferimento a questi valori per comprendere meglio gli esempi di questo articolo:

```
#Example values

VnetName                = TestVNet1 
ResourceGroup           = TestRG1 
Location                = eastus 
AddressSpace            = 10.12.0.0/16 
SubnetName              = Subnet1 
Subnet                  = 10.12.0.0/24 
GatewaySubnet           = 10.12.255.0/27 
LocalNetworkGatewayName = Site2 
LNG Public IP           = <VPN device IP address>
LocalAddrPrefix1        = 10.0.0.0/24 
LocalAddrPrefix2        = 20.0.0.0/24   
GatewayName             = VNet1GW 
PublicIP                = VNet1GWIP 
VPNType                 = RouteBased 
GatewayType             = Vpn 
ConnectionName          = VNet1toSite2
```

## <a name="Login"></a>1. Accedere ad Azure

Accedere alla sottoscrizione di Azure con il comando [az login](/cli/azure/#login) e seguire le istruzioni visualizzate.

```azurecli
az login
```

Se si hanno più sottoscrizioni Azure, elencare le sottoscrizioni per l'account.

```azurecli
Az account list --all
```

Specificare la sottoscrizione da usare.

```azurecli
Az account set --subscription <replace_with_your_subscription_id>
```

## <a name="2-create-a-resource-group"></a>2. Creare un gruppo di risorse

L'esempio seguente crea un gruppo di risorse denominato "TestRG1" nella località "eastus". Se è già disponibile un gruppo di risorse nell'area in cui si vuole creare la rete virtuale, è possibile usarlo.

```azurecli
az group create -n TestRG1 -l eastus
```

## <a name="VNet"></a>3. Crea rete virtuale

Se non si ha una rete virtuale, crearne una. Quando si crea una rete virtuale, verificare che gli spazi di indirizzi specificati non si sovrappongano agli spazi di indirizzi presenti nella rete locale. 

L'esempio seguente crea una rete virtuale denominata "TestVNet1" e una subnet "Subnet1".

```azurecli
az network vnet create -n TestVNet1 -g TestRG1 --address-prefix 10.12.0.0/16 -l eastus --subnet-name Subnet1 --subnet-prefix 10.12.0.0/24
```

## 4. <a name="gwsub"></a>Creare la subnet del gateway

[!INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]

Per questa configurazione, è necessaria anche una subnet del gateway. Il gateway di rete virtuale usa una subnet del gateway che contiene gli indirizzi IP usati dai servizi del gateway VPN. Quando si crea una subnet del gateway, deve essere denominata "GatewaySubnet". Se si assegna un nome diverso, la subnet viene creata, ma non verrà trattata da Azure come una subnet del gateway.

Le dimensioni della subnet del gateway specificata dipendono dalla configurazione del gateway VPN che si vuole creare. Nonostante sia possibile creare una subnet del gateway con dimensioni di /29 soltanto, è consigliabile crearne una più grande che includa più indirizzi selezionando /27 o /28. L'uso di una subnet del gateway di dimensioni maggiori consente un numero sufficiente di indirizzi IP per eventuali configurazioni future.


```azurecli
az network vnet subnet create --address-prefix 10.12.255.0/27 -n GatewaySubnet -g TestRG1 --vnet-name TestVNet1
```

## <a name="localnet"></a>5. Creare il gateway di rete locale

Il gateway di rete locale in genere fa riferimento al percorso locale. Assegnare al sito un nome che Azure possa usare come riferimento, quindi specificare l'indirizzo IP del dispositivo VPN locale con cui si creerà una connessione. Specificare anche i prefissi degli indirizzi IP che verranno instradati tramite il gateway VPN al dispositivo VPN. I prefissi degli indirizzi specificati sono quelli disponibili nella rete locale. Se la rete locale viene modificata, è possibile aggiornare facilmente i prefissi.

Usare i valori seguenti:

* *--gateway-ip-address* è l'indirizzo IP del dispositivo VPN locale. Il dispositivo VPN non può trovarsi dietro un NAT.
* *--local-address-prefixes* sono gli spazi di indirizzi locali.

L'esempio seguente illustra come aggiungere un gateway di rete locale con più prefissi di indirizzo:

```azurecli
az network local-gateway create --gateway-ip-address 23.99.221.164 -n Site2 -g TestRG1 --local-address-prefixes 10.0.0.0/24 20.0.0.0/24
```

## <a name="PublicIP"></a>6. Richiedere un indirizzo IP pubblico

Richiedere un indirizzo IP pubblico che verrà allocato per il gateway VPN di rete virtuale. Si tratta dell'indirizzo IP configurato per la connessione del dispositivo VPN.

Il gateway di rete virtuale per il modello di distribuzione Resource Manager supporta attualmente solo indirizzi IP pubblici usando il metodo di allocazione dinamica. Ciò non significa però che l'indirizzo IP verrà modificato. L'indirizzo IP del gateway VPN viene modificato solo quando il gateway viene eliminato e ricreato. L'indirizzo IP pubblico del gateway di rete virtuale non cambia in caso di ridimensionamento, reimpostazione o altri aggiornamenti o manutenzioni interne del gateway VPN. 

```azurecli
az network public-ip create -n VNet1GWIP -g TestRG1 --allocation-method Dynamic
```

## <a name="CreateGateway"></a>7. Creare il gateway VPN

Creare il gateway VPN di rete virtuale. Per completare la creazione di un gateway VPN, possono essere necessari fino a 45 minuti o più.

Usare i valori seguenti:

* Il valore di *--gateway-type* per una configurazione da sito a sito è *Vpn*. Il tipo di gateway è sempre specifico della configurazione che si sta implementando. Per altre informazioni, vedere [Tipi di gateway](vpn-gateway-about-vpn-gateway-settings.md#gwtype).
* Il valore di *--vpn-type* può essere *RouteBased*, talvolta definito gateway dinamico nella documentazione, o *PolicyBased*, talvolta definito gateway statico nella documentazione. L'impostazione è specifica dei requisiti del dispositivo a cui ci si connette. Per altre informazioni sui tipi di gateway VPN, vedere [Informazioni sulle impostazioni di configurazione del gateway VPN](vpn-gateway-about-vpn-gateway-settings.md#vpntype).
* Il valore di *--sku* può essere Basic, Standard o HighPerformance. Esistono limitazioni di configurazione per alcuni SKU. Per altre informazioni, vedere [SKU del gateway](vpn-gateway-about-vpngateways.md#gateway-skus).

Dopo avere eseguito questo comando, non verranno visualizzati commenti o output. La creazione di un gateway richiede circa 45 minuti.

```azurecli
az network vnet-gateway create -n VNet1GW --public-ip-address VNet1GWIP -g TestRG1 --vnet TestVNet1 --gateway-type Vpn --vpn-type RouteBased --sku Standard --no-wait 
```

## <a name="VPNDevice"></a>8. Configurare il dispositivo VPN

[!INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]
 Per trovare l'indirizzo IP pubblico del gateway di rete virtuale, usare l'esempio seguente, sostituendo i valori con quelli personalizzati. Per facilitare la lettura, la formattazione dell'output visualizza l'elenco di IP pubblici in formato tabella.

  ```azurecli
  az network public-ip list -g TestRG1 -o table
  ```

## <a name="CreateConnection"></a>9. Creare la connessione VPN

Creare la connessione VPN da sito a sito tra il gateway di rete virtuale e il dispositivo VPN locale. Prestare particolare attenzione al valore della chiave condivisa, che deve corrispondere al valore della chiave condivisa configurato per il dispositivo VPN.

```azurecli
az network vpn-connection create -n VNet1toSite2 -g TestRG1 --vnet-gateway1 VNet1GW -l eastus --shared-key abc123 --local-gateway2 Site2
```

La connessione verrà stabilita in breve tempo.

## <a name="toverify"></a>10. Verificare la connessione VPN

[!INCLUDE [verify connection](../../includes/vpn-gateway-verify-connection-cli-rm-include.md)] 

Per usare un altro metodo per verificare la connessione, vedere [Verificare una connessione di Gateway VPN](vpn-gateway-verify-connection-resource-manager.md).

## <a name="common-tasks"></a>Attività comuni

### <a name="to-view-local-network-gateways"></a>Per visualizzare i gateway di rete locali

```azurecli
az network local-gateway list --resource-group TestRG1
```

### <a name="modify"></a>Per modificare i prefissi di indirizzo IP per un gateway di rete locale
Se è necessario modificare i prefissi per il gateway di rete locale, usare le istruzioni seguenti. Ogni volta che si apporta una modifica, deve essere specificato l'intero elenco di prefissi, non solo quelli che si vuole modificare.

- **Se è stata specificata una connessione**, usare l'esempio seguente. Specificare l'intero elenco di prefissi comprendente i prefissi esistenti e quelli che si vuole aggiungere. In questo esempio 10.0.0.0/24 e 20.0.0.0/24 sono già presenti. Vengono aggiunti i prefissi 30.0.0.0/24 e 40.0.0.0/24.

  ```azurecli
  az network local-gateway update --local-address-prefixes 10.0.0.0/24 20.0.0.0/24 30.0.0.0/24 40.0.0.0/24 -n VNet1toSite2 -g TestRG1
  ```

- **Se non è stata specificata una connessione**, usare lo stesso comando usato per creare un gateway di rete locale. È anche possibile usare questo comando per aggiornare l'indirizzo IP del gateway per il dispositivo VPN. Usare questo comando solo quando non si ha già una connessione. In questo esempio 10.0.0.0/24, 20.0.0.0/24, 30.0.0.0/24 e 40.0.0.0/24 sono presenti. Vengono specificati solo i prefissi che si vuole mantenere. In questo caso, 10.0.0.0/24 e 20.0.0.0/24.

  ```azurecli
  az network local-gateway create --gateway-ip-address 23.99.221.164 -n Site2 -g TestRG1 --local-address-prefixes 10.0.0.0/24 20.0.0.0/24
  ```

### <a name="modifygwipaddress"></a>Per modificare l'indirizzo IP del gateway per un gateway di rete locale

L'indirizzo IP in questa configurazione è l'indirizzo IP del dispositivo VPN a cui si sta creando una connessione. Se viene modificato l'indirizzo IP del dispositivo VPN, è possibile modificare il valore. L'indirizzo IP può essere modificato anche se è disponibile una connessione gateway.

```azurecli
az network local-gateway update --gateway-ip-address 23.99.222.170 -n Site2 -g TestRG1
```

Quando si visualizza il risultato, verificare che siano inclusi i prefissi degli indirizzi IP.

  ```azurecli
  "localNetworkAddressSpace": { 
    "addressPrefixes": [ 
      "10.0.0.0/24", 
      "20.0.0.0/24", 
      "30.0.0.0/24" 
    ] 
  }, 
  "location": "eastus", 
  "name": "Site2", 
  "provisioningState": "Succeeded",  
  ```

### <a name="to-view-the-virtual-network-gateway-public-ip-address"></a>Per visualizzare l'indirizzo IP pubblico del gateway di rete virtuale

Per trovare l'indirizzo IP pubblico del gateway di rete virtuale, usare l'esempio seguente. Per facilitare la lettura, la formattazione dell'output visualizza l'elenco di IP pubblici in formato tabella.

```azurecli
az network public-ip list -g TestRG1 -o table
```

### <a name="to-verify-the-shared-key-values"></a>Per verificare i valori della chiave condivisa

Verificare che il valore della chiave condivisa sia lo stesso usato per la configurazione del dispositivo VPN. In caso contrario, eseguire di nuovo la connessione usando il valore del dispositivo o aggiornare il dispositivo con il valore restituito. I valori devono corrispondere.

```azurecli
az network vpn-connection shared-key show --connection-name VNet1toSite2 -g TestRG1
```

## <a name="next-steps"></a>Passaggi successivi

*  Dopo aver completato la connessione, è possibile aggiungere macchine virtuali alle reti virtuali. Per altre informazioni, vedere [Macchine virtuali](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).
* Per informazioni su BGP, vedere [Panoramica di BGP](vpn-gateway-bgp-overview.md) e [Come configurare BGP](vpn-gateway-bgp-resource-manager-ps.md).
* Per informazioni sul tunneling forzato, vedere [Configurare il tunneling forzato](vpn-gateway-forced-tunneling-rm.md).
* Per un elenco di comandi dell'interfaccia della riga di comando di Azure di rete, vedere [Azure CLI](https://docs.microsoft.com/cli/azure/network) (Interfaccia della riga di comando di Azure).
