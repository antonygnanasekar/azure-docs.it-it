---
title: 'Panoramica del gateway VPN: creare connessioni VPN cross-premise alle reti virtuali di Azure | Documentazione Microsoft'
description: Questa panoramica del gateway VPN illustra i modi per connettersi alle reti virtuali di Azure usando una connessione VPN tramite Internet. Sono inclusi i diagrammi delle configurazioni della connessione di base.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 2358dd5a-cd76-42c3-baf3-2f35aadc64c8
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/04/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 6ea03adaabc1cd9e62aa91d4237481d8330704a1
ms.openlocfilehash: 1dfd9be47adfcb6de46f26571847a3a6b7f51410
ms.lasthandoff: 04/06/2017


---
# <a name="about-vpn-gateway"></a>Informazioni sul gateway VPN
Per inviare il traffico di rete tra la rete virtuale di Azure e il sito locale, è necessario creare un gateway di rete virtuale per la rete virtuale. Un gateway VPN è un tipo di gateway di rete virtuale che invia traffico crittografato tramite una connessione pubblica. È anche possibile usare i gateway VPN per inviare il traffico tra le reti virtuali di Azure tramite la rete Microsoft.

Esistono due tipi di gateway di rete virtuale: "ExpressRoute" e "Vpn". Quando si crea un gateway di rete virtuale, si specifica il tipo di gateway che si vuole creare. Un gateway VPN è un gateway di rete virtuale che usa il tipo di gateway "Vpn". 

Ogni rete virtuale può avere due gateway di rete virtuale, ma solo uno per ogni tipo. A seconda delle impostazioni scelte, è possibile creare più connessioni a un singolo gateway VPN. Una configurazione di connessione multisito ne è un esempio. Quando si creano più connessioni allo stesso gateway VPN, tutti i tunnel VPN, incluse le VPN da punto a sito, condividono la larghezza di banda disponibile per il gateway.

## <a name="configuring-a-vpn-gateway"></a>Configurazione di un gateway VPN
Una connessione gateway VPN si basa su più risorse configurate con impostazioni specifiche. La maggior parte delle risorse può essere configurata separatamente, anche se in alcuni casi è necessario configurarle seguendo un determinato ordine.

###<a name="settings"></a>Impostazioni
Le impostazioni scelte per ogni risorsa sono fondamentali per creare una connessione corretta. Per informazioni sulle singole risorse e le impostazioni per il gateway VPN, vedere [Informazioni sulle impostazioni del gateway VPN](vpn-gateway-about-vpn-gateway-settings.md). Si troveranno informazioni su tipi di gateway, tipi di VPN, tipi di connessione, subnet del gateway, gateway di rete locale e diverse altre impostazioni delle risorse che potrebbero essere utili.

###<a name="deployment-tools"></a>Strumenti di distribuzione
È possibile iniziare a creare e configurare le risorse usando uno strumento di configurazione, ad esempio il portale di Azure, e successivamente decidere di passare a un altro strumento, ad esempio PowerShell, per configurare risorse aggiuntive o eventualmente modificare quelle esistenti. Attualmente, non è possibile configurare tutte le risorse e le relative impostazioni nel portale di Azure. Le istruzioni riportate negli articoli per ogni topologia di connessione indicano se è necessario usare uno strumento di configurazione specifico. 

###<a name="deployment-model"></a>Modello di distribuzione
Quando si configura un gateway VPN, i passaggi da eseguire dipendono dal modello di distribuzione usato per creare la rete virtuale. Ad esempio, se la rete virtuale è stata creata usando il modello di distribuzione classica, per creare e configurare le impostazioni del gateway VPN si usano le linee guida e le istruzioni per il modello di distribuzione classica. Per altre informazioni sui modelli di distribuzione, vedere [Confronto tra distribuzione di Azure Resource Manager e classica: comprensione dei modelli di implementazione e dello stato delle risorse](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="diagrams"></a>Diagrammi delle topologie di connessione
È importante tenere presente che sono disponibili configurazioni diverse per le connessioni del gateway VPN. È necessario determinare la configurazione più adatta alle proprie esigenze. Nelle sezioni seguenti è possibile trovare informazioni e diagrammi delle topologie sulle connessioni gateway VPN seguenti. Le sezioni seguenti contengono tabelle che elencano:

* Modello di distribuzione disponibile
* Strumenti di configurazione disponibili
* Collegamenti che visualizzano direttamente un articolo, se disponibile

Usare i diagrammi e le descrizioni per selezionare la topologia di connessione più adatta alle esigenze. I diagrammi illustrano le principali topologie di base, ma è possibile creare configurazioni più complesse usando i diagrammi come riferimento.

## <a name="site-to-site-and-multi-site-ipsecike-vpn-tunnel"></a>Da sito a sito e multisito (tunnel VPN IPsec/IKE)
### <a name="S2S"></a>Da sito a sito
Una connessione gateway VPN da sito a sito (S2S) avviene tramite un tunnel VPN IPsec/IKE (IKEv1 o IKEv2). Questo tipo di connessione richiede un dispositivo VPN che si trova in locale con un indirizzo IP pubblico assegnato e non dietro una NAT. Le connessioni S2S possono essere usate per le configurazioni cross-premise e ibride.   

![Esempio di connessione gateway VPN di Azure da sito a sito](./media/vpn-gateway-about-vpngateways/vpngateway-site-to-site-connection-diagram.png)

### <a name="Multi"></a>Multisito
Questo tipo di connessione è una variante della connessione da sito a sito. È possibile creare più di una connessione VPN dal gateway di rete virtuale, che in genere connette a più siti locali. Quando si usano più connessioni, è necessario usare una rete VPN di tipo RouteBased (nota come gateway dinamico nell'ambito delle reti virtuali classiche). Poiché ogni rete virtuale può avere un solo gateway VPN, tutte le connessioni che usano il gateway condividono la larghezza di banda disponibile. Questo tipo di connessione è spesso definito "multisito".

![Esempio di connessione gateway VPN di Azure multisito](./media/vpn-gateway-about-vpngateways/vpngateway-multisite-connection-diagram.png)

### <a name="deployment-models-and-methods-for-site-to-site-and-multi-site"></a>Metodi e modelli di distribuzione per connessioni da sito a sito e multisito
[!INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)]

## <a name="P2S"></a>Da punto a sito (VPN su SSTP)
Una connessione gateway VPN da punto a sito (P2S) consente di creare una connessione sicura alla rete virtuale da un singolo computer client. P2S è una connessione VPN tramite SSTP (Secure Sockets Tunneling Protocol). Le connessioni P2S non richiedono un dispositivo VPN o un indirizzo IP pubblico per funzionare. Per stabilire la connessione VPN, avviarla dal computer client. Questa è la soluzione ideale quando ci si vuole connettere alla rete virtuale da una posizione remota, ad esempio da casa o durante una riunione, oppure quando solo pochi client devono connettersi a una rete virtuale. Le connessioni da punto a sito possono essere usate con le connessioni da sito a sito attraverso lo stesso gateway VPN, purché tutti i requisiti di configurazione per entrambe le connessioni siano compatibili.

![Esempio di connessione gateway VPN di Azure da punto a sito](./media/vpn-gateway-about-vpngateways/vpngateway-point-to-site-connection-diagram.png)

### <a name="deployment-models-and-methods-for-point-to-site"></a>Metodi e modelli di distribuzione per connessioni da punto a sito
[!INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="V2V"></a>Connessioni da rete virtuale a rete virtuale (tunnel VPN IPsec/IKE)
La connessione di una rete virtuale a un'altra rete virtuale (da rete virtuale a rete virtuale) è simile alla connessione di una rete virtuale a un percorso di sito locale. Entrambi i tipi di connettività utilizzano un gateway VPN per fornire un tunnel sicuro tramite IPsec/IKE. È anche possibile combinare una comunicazione tra reti virtuali con configurazioni di connessioni multisito. Questo permette di definire topologie di rete che consentono di combinare la connettività cross-premise con la connettività tra reti virtuali.

Le reti virtuali connesse possono essere:

* Nella stessa area o in aree diverse
* Nella stessa sottoscrizione o in sottoscrizioni diverse 
* Nello stesso modello di distribuzione o in modelli diversi

![Esempio di connessione gateway VPN di Azure da rete virtuale a rete virtuale](./media/vpn-gateway-about-vpngateways/vpngateway-vnet-to-vnet-connection-diagram.png)

###<a name="connections-between-deployment-models"></a>Connessioni tra modelli di distribuzione
Azure offre attualmente di due modelli di distribuzione: classica e Resource Manager. Se si usa Azure da qualche tempo, si dispone probabilmente di VM e istanze del ruolo di Azure in esecuzione su una rete virtuale classica. Le VM e le istanze del ruolo più recenti potrebbero invece essere in esecuzione su una rete virtuale creata in Resource Manager. Si crea una connessione tra le reti virtuali per consentire alle risorse di una rete virtuale di comunicare direttamente con le risorse di un'altra.

###<a name="vnet-peering"></a>Peering reti virtuali
È possibile usare il peering reti virtuali per creare la connessione, purché la rete virtuale soddisfi determinati requisiti. Peering reti virtuali non usa un gateway di rete virtuale. Per altre informazioni, vedere [Peering reti virtuali](../virtual-network/virtual-network-peering-overview.md).

###<a name="deployment-models-and-methods-for-vnet-to-vnet"></a>Metodi e modelli di distribuzione per connessioni da rete virtuale a rete virtuale
[!INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

## <a name="ExpressRoute"></a>ExpressRoute (connessione privata dedicata)
Microsoft Azure ExpressRoute consente di estendere le reti locali nel cloud Microsoft tramite una connessione privata dedicata fornita da un provider di connettività. Con ExpressRoute è possibile stabilire connessioni ai servizi cloud Microsoft, come Microsoft Azure, Office 365 e CRM Online. La connettività può essere stabilita da una rete (IP VPN) any-to-any, da una rete Ethernet punto a punto o da una Cross Connection virtuale tramite un provider di connettività presso una struttura di condivisione del percorso.

Le connessioni ExpressRoute non sfruttano la rete Internet pubblica. In questo modo possono offrire un livello di sicurezza superiore, maggiore affidabilità, velocità più elevate e minori latenze rispetto alle connessioni Internet tradizionali.

Una connessione ExpressRoute non usa un gateway VPN, anche se usa un gateway di rete virtuale come parte della configurazione obbligatoria. In una connessione ExpressRoute il gateway di rete virtuale viene configurato con il tipo di gateway "ExpressRoute" invece che "Vpn". Per altre informazioni su ExpressRoute, vedere [Panoramica tecnica relativa a ExpressRoute](../expressroute/expressroute-introduction.md).

## <a name="coexisting"></a>Connessioni coesistenti da sito a sito ed ExpressRoute
ExpressRoute è una connessione dedicata diretta dalla rete WAN (non sulla rete Internet pubblica) a servizi Microsoft come Azure. Il traffico VPN da sito a sito viaggia crittografato sulla rete Internet pubblica. La possibilità di configurare connessioni VPN da sito a sito ed ExpressRoute per la stessa rete virtuale offre diversi vantaggi.

È possibile configurare una VPN da sito a sito come percorso di failover sicuro per ExpressRoute oppure usare VPN da sito a sito per connettersi a siti che non fanno parte della rete, ma che sono connessi tramite ExpressRoute. Si noti che questa configurazione richiede due gateway di rete virtuale per la stessa rete virtuale, uno che usa il tipo di gateway "Vpn" e l'altro che usa il tipo di gateway "ExpressRoute".

![Esempio di connessioni coesistenti ExpressRoute e gateway VPN](./media/vpn-gateway-about-vpngateways/expressroute-vpngateway-coexisting-connections-diagram.png)

### <a name="deployment-models-and-methods-for-s2s-and-expressroute"></a>Metodi e modelli di distribuzione per le connessioni da sito a sito ed ExpressRoute
[!INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)]

## <a name="pricing"></a>Prezzi
[!INCLUDE [vpn-gateway-about-pricing-include](../../includes/vpn-gateway-about-pricing-include.md)]

## <a name="gateway-skus"></a>SKU del gateway
[!INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

Per informazioni sugli SKU del gateway VPN, vedere [SKU del gateway](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

### <a name="estimated-aggregate-throughput-by-sku"></a>Velocità effettiva aggregata stimata per SKU
[!INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]

## <a name="next-steps"></a>Passaggi successivi
- Pianificare la configurazione del gateway VPN. Vedere [Pianificazione e progettazione per il gateway VPN](vpn-gateway-plan-design.md).
- Per altre informazioni, vedere [Domande frequenti sul gateway VPN](vpn-gateway-vpn-faq.md).
- Vedere [Sottoscrizione e limiti del servizio](../azure-subscription-service-limits.md#networking-limits).


