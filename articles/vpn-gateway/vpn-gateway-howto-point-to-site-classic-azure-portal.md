---
title: 'Connettere un computer a una rete virtuale di Azure da punto a sito: portale di Azure: distribuzione classica | Documentazione Microsoft'
description: Connettersi in modo sicuro a una rete virtuale di Azure classica creando una connessione gateway VPN da punto a sito con il portale di Azure.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 65e14579-86cf-4d29-a6ac-547ccbd743bd
ms.service: vpn-gateway
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/10/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 3d8806411f775d1e6e69af66326534f69c468771
ms.lasthandoff: 04/12/2017


---
# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal-classic"></a>Configurare una connessione da punto a sito a una rete virtuale usando il portale di Azure (versione classica)
> [!div class="op_single_selector"]
> * [Resource Manager - Portale di Azure](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Resource Manager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Classica - Portale di Azure](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
>
>

Una configurazione da punto a sito (P2S) permette di creare una connessione protetta da un singolo computer client a una rete virtuale. P2S è una connessione VPN tramite SSTP (Secure Sockets Tunneling Protocol). Una soluzione Da punto a sito è utile quando ci si vuole connettere alla rete virtuale da una posizione remota, ad esempio da casa o durante una riunione, oppure quando solo pochi client devono connettersi a una rete virtuale. Le connessioni P2S non richiedono un dispositivo VPN o un indirizzo IP pubblico. La connessione VPN viene avviata dal computer client.

Questo articolo illustra la creazione di una rete virtuale con una connessione da punto a sito nel modello di distribuzione classica usando il portale di Azure. Per altre informazioni sulla connettività da punto a sito, consultare le [Domande frequenti sulla connettività da punto a sito](#faq) alla fine di questo articolo.


### <a name="deployment-models-and-methods-for-p2s-connections"></a>Metodi e modelli di distribuzione per connessioni P2S
[!INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

La tabella seguente descrive i due modelli di distribuzione e i metodi di distribuzione disponibili per le configurazioni P2S. Quando è disponibile un articolo contenente la procedura di configurazione, nella tabella è presente un collegamento diretto.

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)]

## <a name="basic-workflow"></a>Flusso di lavoro di base
![Diagramma da punto a sito](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/point-to-site-connection-diagram.png)

Le sezioni seguenti illustrano i passaggi per creare una connessione da punto a sito sicura a una rete virtuale.

1. Creare una rete virtuale e un gateway VPN
2. Generare i certificati
3. Caricare il file CER
4. Generare il pacchetto di configurazione del client VPN
5. Configurare il computer client
6. Connettersi ad Azure

### <a name="example-settings"></a>Impostazioni di esempio
È possibile usare le impostazioni di esempio seguenti.

* **Nome: VNet1**
* **Spazio indirizzi: 192.168.0.0/16**<br>Per questo esempio, viene usato un solo spazio indirizzi. È possibile avere più di uno spazio indirizzi per la rete virtuale.
* **Nome subnet: FrontEnd**
* **Intervallo di indirizzi subnet: 192.168.1.0/24**
* **Sottoscrizione:** se si hanno più sottoscrizioni, verificare di usare quella corretta.
* **Gruppo di risorse: TestRG**
* **Località: Stati Uniti orientali**
* **Tipo di connessione: Da punto a sito**
* **: 172.16.201.0/24**. I client VPN che si connettono alla rete virtuale con questa connessione da punto a sito ricevono un indirizzo IP dal pool specificato.
* **GatewaySubnet: 192.168.200.0/24**. La subnet del gateway deve usare il nome "GatewaySubnet".
* **Dimensioni:** selezionare lo SKU di gateway da usare.
* **Tipo di routing: Dinamico**



## <a name="vnetvpn"></a>Sezione 1: Creare una rete virtuale e un gateway VPN

Prima di iniziare, verificare di possedere una sottoscrizione di Azure. Se non si ha una sottoscrizione di Azure, è possibile attivare i [vantaggi per i sottoscrittori di MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) oppure iscriversi per ottenere un [account gratuito](https://azure.microsoft.com/pricing/free-trial).
### <a name="createvnet"></a>Parte 1: Creare una rete virtuale
Se non si ha una rete virtuale, crearne una. Gli screenshot sono forniti come esempio. Assicurarsi di sostituire i valori con i propri. Per creare una rete virtuale usando il portale di Azure, seguire questa procedura:

1. In un browser passare al [portale di Azure](http://portal.azure.com) e, se necessario, accedere con l'account Azure.
2. Fare clic su **New**. Nel campo **Cerca nel Marketplace** digitare "Rete virtuale". Trovare **Rete virtuale** nell'elenco restituito e fare clic per aprire il pannello **Rete virtuale**.

    ![Cercare il pannello Rete virtuale](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvnetportal700.png)
3. Nella parte inferiore del pannello Rete virtuale selezionare **Classico** nell'elenco **Selezionare un modello di distribuzione** e quindi fare clic su **Crea**.

    ![Selezionare il modello di distribuzione](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/selectmodel.png)
4. Nel pannello **Crea rete virtuale** configurare le impostazioni della rete virtuale. Aggiungere in questo pannello il primo spazio di indirizzi e un intervallo di indirizzi di una singola subnet. Dopo aver creato la rete virtuale, è possibile tornare indietro e aggiungere altre subnet e altri spazi di indirizzi.

    ![Pannello Creare la rete virtuale](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vnet125.png)
5. Verificare che la **sottoscrizione** sia quella corretta. È possibile cambiare sottoscrizione tramite l'elenco a discesa.
6. Fare clic su **Gruppo di risorse** e selezionare un gruppo di risorse esistente o crearne uno nuovo digitandone il nome. Se si crea un nuovo gruppo di risorse, denominare il gruppo in base ai valori di configurazione pianificati. Per altre informazioni sui gruppi di risorse, vedere [Panoramica di Azure Resource Manager](../azure-resource-manager/resource-group-overview.md#resource-groups).
7. Selezionare quindi le impostazioni relative alla **località** per la rete virtuale. La località determina la posizione in cui risiedono le risorse distribuite in questa rete virtuale.
8. Selezionare **Aggiungi al dashboard** se si vuole trovare facilmente la rete virtuale nel dashboard e quindi fare clic su **Crea**.

    ![Aggiungi al dashboard](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/pintodashboard150.png)
9. Dopo aver fatto clic su Crea, nel dashboard viene visualizzato un riquadro che riflette lo stato di avanzamento della rete virtuale. Il riquadro cambia durante la creazione della rete virtuale.

    ![Pannello Creazione della rete virtuale](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png)
10. Dopo aver creato la rete virtuale, è possibile aggiungere l'indirizzo IP di un server DNS per gestire la risoluzione dei nomi. Aprire le impostazioni della rete virtuale, fare clic su Server DNS e aggiungere l'indirizzo IP del server DNS da usare. Questa impostazione non determina la creazione di un nuovo server DNS. Assicurarsi di aggiungere un server DNS con cui le risorse possono comunicare.

Al termine della creazione della rete virtuale, nella pagina Reti del portale di Azure classico verrà visualizzato **Creato** in **Stato**.

### <a name="gateway"></a>Parte 2: Creare una subnet del gateway e un gateway di routing dinamico
In questo passaggio si creeranno una subnet del gateway e un gateway di routing dinamico. Nel portale di Azure per il modello di distribuzione classica, la subnet del gateway e il gateway possono essere creati tramite gli stessi pannelli di configurazione.

1. Nel portale passare alla rete virtuale per cui si vuole creare un gateway.
2. Nel pannello **Panoramica** del pannello della rete virtuale fare clic su **Gateway** nella sezione Connessioni VPN.

    ![Fare clic per creare un gateway](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/beforegw125.png)
3. Nel pannello **Nuova connessione VPN** selezionare **Da punto a sito**.

    ![Tipo di connessione da punto a sito](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvpnconnect.png)
4. In **Spazio indirizzi client** aggiungere l'intervallo di indirizzi IP, da cui i client VPN ricevono un indirizzo IP al momento della connessione. Eliminare l'intervallo compilato automaticamente e aggiungere il proprio.

    ![Spazio indirizzi client](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientaddress.png)
5. Selezionare la casella di controllo **Crea gateway immediatamente**.

    ![Crea gateway immediatamente](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/creategwimm.png)
6. Fare clic su **Configurazione gateway facoltativa** per aprire il pannello **Configurazione gateway**.

    ![Fare clic su Configurazione gateway facoltativa](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/optsubnet125.png)
7. Fare clic su **Subnet Configure required settings** (Configura impostazioni necessarie subnet) per aggiungere la **subnet del gateway**. Nonostante sia possibile creare una subnet del gateway con dimensioni di /29 soltanto, è consigliabile crearne una più grande che includa più indirizzi selezionando almeno /28 o /27. In questo modo saranno disponibili indirizzi sufficienti per supportare in futuro le possibili configurazioni aggiuntive desiderate.

   > [!IMPORTANT]
   > Quando si usano le subnet del gateway, evitare di associare un gruppo di sicurezza di rete (NSG) alla subnet del gateway. Se si associa un gruppo di sicurezza di rete a tale subnet, il gateway VPN potrebbe smettere di funzionare come previsto. Per altre informazioni sui gruppi di sicurezza di rete, vedere [Che cos'è un gruppo di sicurezza di rete](../virtual-network/virtual-networks-nsg.md).
   >
   >

    ![Aggiungere GatewaySubnet](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsubnet125.png)
8. Selezionare le **dimensioni** del gateway, che rappresentano lo SKU di gateway per il gateway di rete virtuale. Nel portale, lo SKU predefinito è **Basic**. Per altre informazioni sugli SKU di gateway, vedere [Informazioni sulle impostazioni del gateway VPN](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

    ![Dimensioni del gateway](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsize125.png)
9. Selezionare il **tipo di routing** per il gateway. Le configurazioni P2S richiedono il tipo di routing **Dinamico**. Al termine della configurazione del pannello, fare clic su **OK**.

    ![Configurare il tipo di routing](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/routingtype125.png)
10. Nel pannello **Nuova connessione VPN** fare clic su **OK** nella parte inferiore per iniziare a creare il gateway della rete virtuale. Per completare un gateway VPN possono essere necessari fino a 45 minuti.

## <a name="generatecerts"></a>Sezione 2 - Creare certificati
I certificati vengono usati da Azure per autenticare i client VPN per VPN da punto a sito. Dopo avere creato il certificato radice, esportare i dati del certificato pubblico, non la chiave privata, come file CER X.509 con codifica in base 64. Si caricano quindi i dati del certificato pubblico dal certificato radice ad Azure.

Ogni computer client che si connette a una rete virtuale usando la soluzione Da punto a sito deve avere un certificato client installato. Il certificato client viene generato dal certificato radice e viene installato in ogni computer client. Se non è stato installato un certificato client valido e il client prova a connettersi alla rete virtuale, l'autenticazione avrà esito negativo.

### <a name="cer"></a>Parte 1: Ottenere il file di chiave pubblica (con estensione cer) per il certificato radice

#### <a name="enterprise-certificate"></a>Certificato aziendale
 
Se si tratta di una soluzione aziendale, è possibile usare la catena di certificati esistente. Ottenere il file con estensione cer per il certificato radice che si vuole usare.

#### <a name="self-signed-root-certificate"></a>Certificato radice autofirmato

Se non si usa una soluzione aziendale per la creazione di certificati, è necessario creare un certificato radice autofirmato. Per creare un certificato autofirmato contenente i campi necessari per l'autenticazione da punto a sito, è possibile usare PowerShell. Per informazioni dettagliate sulla procedura di creazione di un certificato autofirmato, vedere [Creare un certificato radice autofirmato per connessioni da punto a sito con PowerShell](vpn-gateway-certificates-point-to-site.md).

> [!NOTE]
> In precedenza, makecert era il metodo consigliato per creare certificati radice autofirmati e generare i certificati client per le connessioni da punto a sito. Ora è possibile usare PowerShell per creare questi certificati. Un vantaggio dell'uso di PowerShell è costituito dalla possibilità di creare certificati SHA-2. Per informazioni sui valori necessari, vedere [Creare un certificato autofirmato per connessioni da punto a sito con PowerShell](vpn-gateway-certificates-point-to-site.md).
>
>

#### <a name="to-export-the-public-key-for-a-self-signed-root-certificate"></a>Per esportare la chiave pubblica per un certificato radice autofirmato

Le connessioni da punto a sito richiedono il caricamento della chiave pubblica (file con estensione cer) in Azure. La procedura seguente consente di esportare il file con estensione .cer per il certificato radice autofirmato.

1. Per ottenere un file con estensione cer dal certificato, aprire **Gestire i certificati utente**.
2. Individuare il certificato radice autofirmato "P2SRootCert" in "Certificati - Utente corrente\Personale\Certificati" e fare clic con il pulsante destro del mouse. Scegliere **Tutte le attività** e quindi fare clic su **Esporta** per aprire **Esportazione guidata certificati**.
3. Nella procedura guidata fare clic su **Avanti**. Selezionare **No, non esportare la chiave privata** e quindi fare clic su **Avanti**.
4. Nella pagina **Formato file di esportazione** selezionare **Codificato Base 64 X.509 (.CER)** e quindi fare clic su **Avanti**. 
5. Nella pagina **File da esportare** passare a "C:", creare una sottodirectory denominata "cert" e selezionarla. Assegnare il nome "P2SRootCert.cer" al file del certificato e quindi fare clic su **Salva**. 
6. Fare clic su **Avanti** e quindi su **Fine** per esportare il certificato. Verrà visualizzato il messaggio **Esportazione completata**. Fare clic su **OK** per chiudere la procedura guidata.

### <a name="genclientcert"></a>Parte 2: Generare un certificato client

È possibile generare un certificato univoco per ogni client oppure usare lo stesso certificato per più client. Generare certificati client univoci offre il vantaggio di poter revocare un singolo certificato. In caso contrario, se tutti gli utenti usano lo stesso certificato client e questo deve essere revocato, è necessario generare e installare nuovi certificati per tutti i client che usano tale certificato per l'autenticazione.

#### <a name="enterprise-certificate"></a>Certificato aziendale
- Se si usa una soluzione aziendale per la creazione di certificati, generare un certificato client con il valore di nome comune nel formato "name@yourdomain.com", invece che nel formato "nome dominio\nome utente".
- Verificare che il certificato client sia basato sul modello di certificato "Utente" con "Autenticazione client" come primo elemento nell'elenco d'uso, invece di Accesso smart card e così via. È possibile controllare il certificato facendo doppio clic sul certificato client e aprendo **Dettagli > Utilizzo chiavi avanzato**.

#### <a name="self-signed-root-certificate"></a>Certificato radice autofirmato 
Se si usa un certificato radice autofirmato, vedere [Generare un certificato client usando PowerShell](vpn-gateway-certificates-point-to-site.md#clientcert) per informazioni sulla procedura per generare un certificato client compatibile con le connessioni da punto a sito.

### <a name="exportclientcert"></a>Parte 3: Esportare il certificato client
Se si genera un certificato client da un certificato radice autofirmato usando le istruzioni di [PowerShell](vpn-gateway-certificates-point-to-site.md#clientcert), viene eseguita l'installazione automatica nel computer usato per generarlo. Se si vuole installare un certificato client in un altro computer client, è necessario esportarlo.
 
1. Per esportare un certificato client, aprire **Gestire i certificati utente**. Fare clic con il pulsante destro del mouse sul certificato client da esportare, scegliere **Tutte le attività** e quindi fare clic su **Esporta** per aprire **Esportazione guidata certificati**.
2. Nella procedura guidata, fare clic su **Avanti**, selezionare **Sì, esporta la chiave privata** e quindi fare clic su **Avanti**.
3. Nella pagina **Formato file di esportazione** lasciare selezionate le impostazioni predefinite. Verificare che l'opzione **Se possibile, includi tutti i certificati nel percorso certificazione** sia selezionata per esportare anche le informazioni necessarie del certificato radice. Quindi fare clic su **Next**.
4. Nella pagina **Sicurezza** è necessario proteggere la chiave privata. Se si sceglie di usare una password, assicurarsi di registrare o ricordare quella impostata per questo certificato. Quindi fare clic su **Next**.
5. In **File da esportare** fare clic su **Sfoglia** e passare alla posizione in cui si vuole esportare il certificato. Per **Nome file**, assegnare un nome al file del certificato. Quindi fare clic su **Next**.
6. Fare clic su **Fine** per esportare il certificato.

## <a name="upload"></a>Sezione 3: Caricare il file CER del certificato radice
Dopo che il gateway è stato creato, è possibile caricare il file CER di un certificato radice trusted in Azure. È possibile caricare file per un massimo di 20 certificati radice. Non si può caricare la chiave privata per il certificato radice in Azure. Dopo che è stato caricato, il file CER viene usato da Azure per autenticare i client che si connettono alla rete virtuale.

1. Nella sezione **Connessioni VPN** del pannello della rete virtuale fare clic sull'icona **client** per aprire il pannello **Connessione VPN da punto a sito**.

    ![Client](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png)
2. Nel pannello **Connessione VPN da punto a sito** fare clic su **Gestisci certificato** per aprire il pannello **Certificati**.<br>

    ![Pannello Certificati](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png)<br><br>
3. Nel pannello **Certificati** fare clic su **Carica** per aprire il pannello **Carica certificato**.<br>

    ![Pannello Carica certificato](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/uploadcerts.png)<br>
4. Fare clic sull'icona della cartella per cercare il file CER. Selezionare il file e quindi fare clic su **OK**. Aggiornare la pagina per visualizzare il certificato caricato nel pannello **Certificati**.

    ![Caricamento del certificato](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/upload.png)<br>

## <a name="vpnclientconfig"></a>Sezione 4: Configurare il client
Per connettersi a una rete virtuale con una VPN da punto a sito, in ogni client deve essere installato un pacchetto di configurazione del client VPN. Il pacchetto non installa un client VPN, ma configura il client VPN nativo di Windows con le impostazioni necessarie per la connessione alla rete virtuale. Per l'elenco dei sistemi operativi client supportati, vedere [Domande frequenti sulla connettività da punto a sito](#faq) alla fine di questo articolo.

### <a name="part-1-generate-and-install-the-vpn-client-configuration-package"></a>Parte 1: Generare e installare il pacchetto di configurazione del client VPN
1. Nel pannello **Panoramica** della rete virtuale nel portale di Azure fare clic sull'icona relativa ai client in **Connessioni VPN** per aprire il pannello **Connessione VPN da punto a sito**.
2. Nella parte superiore del pannello **Connessione VPN da punto a sito** fare clic sul pacchetto di download corrispondente al sistema operativo client in cui verrà installato.

   * Per client a 64 bit, selezionare **Client VPN (64 bit)**.
   * Per client a 32 bit, selezionare **Client VPN (32 bit)**.

     ![Scaricare il pacchetto di configurazione del client VPN](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/dlclient.png)<br>
3. Dopo che è stato generato, scaricare e installare il pacchetto nel computer client. Se viene visualizzato un popup SmartScreen, fare clic su **Altre informazioni** e quindi su **Esegui comunque** per installare il pacchetto. È anche possibile salvare il pacchetto per l'installazione su altri computer client.

### <a name="part-2-install-an-exported-client-certificate"></a>Parte 2: Installare un certificato client esportato

Se si vuole creare una connessione da punto a sito da un computer client diverso da quello usato per generare i certificati client, è necessario installare un certificato client. Quando si installa un certificato client, sarà necessaria la password creata quando il certificato client è stato esportato.

1. Individuare e copiare il file *.pfx* nel computer client. Nel computer client, fare doppio clic sul file *.pfx* per installarlo. Lasciare il **Percorso archivio** impostato come **Utente corrente** e quindi fare clic su **Avanti**.
2. Nella pagina **File da importare** non apportare alcuna modifica. Fare clic su **Avanti**.
3. Nella pagina **Protezione della chiave privata** immettere la password per il certificato, se usato, o verificare che l'entità di sicurezza che installa il certificato sia corretta, quindi fare clic su **Avanti**.
4. Nella pagina **Archivio certificati** lasciare la posizione come predefinita e quindi fare clic su **Avanti**.
5. Fare clic su **Fine**. In **Avviso di sicurezza** per l'installazione del certificato fare clic su **Sì**. È possibile fare clic su "Sì" perché il certificato è stato generato. Il certificato è stato importato correttamente.


## <a name="connect"></a>Parte 6: Connettersi ad Azure
### <a name="connect-to-your-vnet"></a>Connettersi alla rete virtuale
1. Per connettersi alla rete virtuale, nel computer client passare alle connessioni VPN e individuare quella creata, che ha lo stesso nome della rete virtuale locale. Fare clic su **Connetti**. È possibile che venga visualizzato un messaggio popup che fa riferimento all'uso del certificato. In questo caso, fare clic su **Continua** per usare privilegi elevati.
2. Nella pagina **Stato connessione** fare clic su **Connetti** per avviare la connessione. Se viene visualizzato un **Seleziona certificato** dello schermo, verificare che il certificato client visualizzato sia quello che si desidera utilizzare per la connessione. In caso contrario, usare la freccia a discesa per selezionare il certificato corretto e quindi fare clic su **OK**.

    ![Connessione del client VPN](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientconnect.png)
3. Verrà stabilita la connessione.

    ![Connessione stabilita](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/connected.png)

In caso di problemi di connessione, effettuare i controlli seguenti:

- Aprire **Gestire i certificati utente** e passare ad **Autorità di certificazione radice attendibili\Certificati**. Verificare che il certificato radice sia incluso nell'elenco. Per il corretto funzionamento dell'autenticazione è necessario che il certificato radice sia presente. Quando si esporta un certificato client con estensione pfx con il valore predefinito "Se possibile, includi tutti i certificati nel percorso certificazione", vengono esportate anche le informazioni del certificato radice. Durante l'installazione del certificato client, nel computer client viene quindi installato anche il certificato radice. 

- Se si usa un certificato che è stato rilasciato tramite una soluzione CA globale e si riscontrano problemi durante l'autenticazione, controllare l'ordine di autenticazione del certificato client. È possibile controllare l'ordine dell'elenco di autenticazione facendo doppio clic sul certificato client e andando in **Dettagli > Utilizzo chiavi avanzato**. Assicurarsi che l'elenco mostri "Autenticazione client" come primo elemento. In caso contrario, è necessario emettere un certificato client in base al modello di utente con l'autenticazione client come primo elemento nell'elenco. 


### <a name="verify-the-vpn-connection"></a>Verificare la connessione VPN
1. Per verificare che la connessione VPN è attiva, aprire un prompt dei comandi con privilegi elevati ed eseguire *ipconfig/all*.
2. Visualizzare i risultati. Si noti che l'indirizzo IP ricevuto è uno degli indirizzi compresi nell'intervallo di indirizzi di connettività da punto a sito specificato al momento della creazione della rete virtuale. I risultati dovrebbero essere simili a quanto segue:

Esempio:

    PPP adapter VNet1:
        Connection-specific DNS Suffix .:
        Description.....................: VNet1
        Physical Address................:
        DHCP Enabled....................: No
        Autoconfiguration Enabled.......: Yes
        IPv4 Address....................: 192.168.130.2(Preferred)
        Subnet Mask.....................: 255.255.255.255
        Default Gateway.................:
        NetBIOS over Tcpip..............: Enabled

 
 In caso di problemi durante la connessione a una macchina virtuale tramite P2S, usare "ipconfig" per controllare l'indirizzo IPv4 assegnato alla scheda Ethernet nel computer da cui viene effettuata la connessione. Se l'indirizzo IP è compreso nell'intervallo di indirizzi della rete virtuale a cui ci si connette o nell'intervallo di indirizzi del pool di indirizzi del client VPN, si verifica la cosiddetta sovrapposizione dello spazio indirizzi. Con questo tipo di sovrapposizione, il traffico di rete non raggiunge Azure e rimane nella rete locale. Se gli spazi indirizzi di rete non si sovrappongono ma non è comunque possibile connettersi alla VM, vedere l'articolo [Risolvere i problemi di connessione con Desktop remoto di una macchina virtuale di Azure](../virtual-machines/windows/troubleshoot-rdp-connection.md).

## <a name="add"></a>Aggiungere o rimuovere certificati radice attendibili

È possibile aggiungere e rimuovere certificati radice attendibili da Azure. Quando si rimuove un certificato attendibile, i certificati client generati dal certificato radice non potranno più connettersi ad Azure tramite una connessione da punto a sito. Per consentire ai client di connettersi, sarà necessario installare un nuovo certificato client generato da un certificato considerato attendibile in Azure.

### <a name="to-add-a-trusted-root-certificate"></a>Per aggiungere un certificato radice attendibile

In Azure è possibile aggiungere fino a 20 file CER di certificato radice trusted. Per istruzioni, vedere [Sezione 3: Caricare il file CER del certificato radice](#upload).

### <a name="to-remove-a-trusted-root-certificate"></a>Per rimuovere un certificato radice attendibile


1. Nella sezione **Connessioni VPN** del pannello della rete virtuale fare clic sull'icona **client** per aprire il pannello **Connessione VPN da punto a sito**.

    ![Client](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png)
2. Nel pannello **Connessione VPN da punto a sito** fare clic su **Gestisci certificato** per aprire il pannello **Certificati**.<br>

    ![Pannello Certificati](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png)<br><br>
3. Nel pannello **Certificati** fare clic sui puntini di sospensione accanto al certificato che si vuole rimuovere e quindi fare clic su **Elimina**.

     ![Eliminare un certificato radice](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deleteroot.png)<br>


## <a name="revokeclient"></a>Revocare un certificato client
È possibile revocare i certificati client. L'elenco di revoche di certificati consente di negare in modo selettivo la connettività da punto a sito basata sui singoli certificati client. Questa operazione è diversa rispetto alla rimozione di un certificato radice attendibile. Se si rimuove un file con estensione cer del certificato radice attendibile da Azure, viene revocato l'accesso per tutti i certificati client generati o firmati dal certificato radice revocato. La revoca di un certificato client, anziché del certificato radice, consente di continuare a usare gli altri certificati generati dal certificato radice per l'autenticazione per la connessione da punto a sito.

La regola generale è quella di usare il certificato radice per gestire l'accesso a livello di team o organizzazione, usando i certificati client revocati per il controllo di accesso con granularità fine su singoli utenti.

### <a name="to-revoke-a-client-certificate"></a>Per revocare un certificato client

È possibile revocare un certificato client aggiungendo l'identificazione personale all'elenco di revoche di certificati.

1. Ottenere l'identificazione personale del certificato client. Per altre informazioni, vedere [Procedura: recuperare l'identificazione personale di un certificato](https://msdn.microsoft.com/library/ms734695.aspx).
2. Copiare le informazioni in un editor di testo e rimuovere tutti gli spazi in modo che sia una stringa continua.
3. Passare al pannello **"nome rete virtuale classica" > Connessione VPN da punto a sito > Certificati** e quindi fare clic su **Elenco di revoche** per aprire il pannello Elenco di revoche. 
4. Nel pannello **Elenco di revoche** fare clic su **+Aggiungi certificato** per aprire il pannello **Aggiungi certificato all'elenco di revoche**.
5. Nel pannello **Aggiungi certificato all'elenco di revoche** incollare l'identificazione personale del certificato come una riga di testo continua, senza spazi. Fare clic su **OK** nella parte inferiore del pannello.
6. Dopo aver completato l'aggiornamento, il certificato non può più essere usato per la connessione. Per i client che tentano di connettersi con questo certificato verrà visualizzato un messaggio che informa che il certificato non è più valido.


## <a name="faq"></a>Domande frequenti sulla connettività da punto a sito

[!INCLUDE [Point-to-Site FAQ](../../includes/vpn-gateway-point-to-site-faq-include.md)]

## <a name="next-steps"></a>Passaggi successivi
Dopo aver completato la connessione, è possibile aggiungere macchine virtuali alle reti virtuali. Per altre informazioni, vedere [Macchine virtuali](https://docs.microsoft.com/azure/#pivot=services&panel=Compute). Per altre informazioni sulla rete e sulle macchine virtuali, vedere [Panoramica di rete delle macchine virtuali Linux e Azure](../virtual-machines/linux/azure-vm-network-overview.md).

