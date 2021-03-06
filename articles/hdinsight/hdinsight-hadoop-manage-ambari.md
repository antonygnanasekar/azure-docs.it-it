---
title: Monitorare e gestire Azure HDInsight con l&quot;interfaccia utente Web Ambari | Documentazione Microsoft
description: Informazioni sull&quot;uso di Ambari per monitorare e gestire cluster HDInsight basati su Linux. Questo documento spiega come usare l&quot;interfaccia utente Web di Ambari inclusa nei cluster HDInsight.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 4787f3cc-a650-4dc3-9d96-a19a67aad046
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/08/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 290271855ac54af8c99311ff675a08d1e6942d93
ms.lasthandoff: 04/12/2017


---
# <a name="manage-hdinsight-clusters-by-using-the-ambari-web-ui"></a>Gestire i cluster HDInsight mediante l'utilizzo dell'interfaccia utente Web Ambari

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari semplifica la gestione e il monitoraggio di un cluster Hadoop grazie a un'interfaccia utente Web facile da usare e alle API REST. Ambari è incluso nei cluster HDInsight basati su Linux e viene usato per monitorare il cluster e modificare la configurazione.

Questo documento spiega come usare l'interfaccia utente Web Ambari con un cluster HDInsight.

## <a id="whatis"></a>Cos'è Ambari?

[Apache Ambari](http://ambari.apache.org) semplifica la gestione di Hadoop grazie a un'interfaccia utente Web intuitiva che può essere usata per effettuare il provisioning, la gestione e il monitoraggio dei cluster Hadoop. Gli sviluppatori possono integrare queste funzionalità nelle applicazioni usando le [API REST Ambari](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

L'interfaccia utente Web Ambari è disponibile per impostazione predefinita con i cluster HDInsight che usano il sistema operativo Linux.

> [!IMPORTANT]
> Linux è l'unico sistema operativo usato in HDInsight versione 3.4 o successiva. Per altre informazioni, vedere [HDInsight deprecato in Windows](hdinsight-component-versioning.md#hdi-version-33-nearing-deprecation-date). 

## <a name="connectivity"></a>Connettività

L'interfaccia utente Web Ambari è disponibile nel cluster HDInsight all'indirizzo HTTPS://CLUSTERNAME.azurehdidnsight.net, dove **CLUSTERNAME** è il nome del cluster.

> [!IMPORTANT]
> La connessione ad Ambari su HDInsight richiede HTTPS. È anche necessario eseguire l'autenticazione in Ambari usando il nome dell'account amministratore (il valore predefinito è **admin**) e la password fornita al momento della creazione del cluster.

## <a name="ssh-tunnel-proxy"></a>Tunnel SSH (proxy)

> [!NOTE]
> Mentre Ambari per il cluster è accessibile direttamente tramite Internet, alcuni collegamenti dall'interfaccia utente Web di Ambari (ad esempio JobTracker) non sono esposte in Internet. Di conseguenza, quando si tenta di accedere a queste funzionalità, verranno visualizzati errori di "server non trovato", a meno di usare un tunnel Secure Shell (SSH) per il proxy del traffico Web al nodo head del cluster.

Per informazioni sulla creazione di un tunnel SSH per lavorare con Ambari, vedere [Usare il tunneling SSH per accedere all'interfaccia Web di Ambari, ResourceManager, JobHistory, NameNode, Oozie e altre interfacce Web](hdinsight-linux-ambari-ssh-tunnel.md).

## <a name="ambari-web-ui"></a>Interfaccia utente Web Ambari

Quando ci si connette all'interfaccia utente Web di Ambari, è necessario eseguire l'autenticazione alla pagina. Utilizzare l'utente di amministrazione del cluster (impostazione predefinita Admin) e la password utilizzati durante la creazione del cluster.

Quando si apre la pagina, si noti la barra in alto, che contiene le informazioni e i controlli seguenti:

![ambari-nav](./media/hdinsight-hadoop-manage-ambari/ambari-nav.png)

* **Logo di Ambari** : apre il dashboard, che può essere usato per monitorare il cluster.

* **Numero di operazioni cluster** : visualizza il numero di operazioni in corso di Ambari. Selezionando il nome del cluster o **# ops**, viene visualizzato un elenco delle operazioni in background.

* **# alerts** : eventuali messaggi di avviso o critici per il cluster. Se si seleziona questa opzione, verrà visualizzato un elenco di avvisi.

* **Dashboard** : visualizza il dashboard.

* **Services** : informazioni e impostazioni di configurazione per i servizi del cluster.

* **Hosts** : informazioni e impostazioni di configurazione per i nodi del cluster.

* **Alerts** : log di informazioni, avvertenze e avvisi critici.

* **Admin** - servizi/stack software che vengono installati nel cluster, informazioni sull'account di servizio e sicurezza Kerberos.

* **Pulsante admin** : gestione di Ambari, impostazioni utente e disconnessione.

## <a name="monitoring"></a>Monitoraggio

### <a name="alerts"></a>Avvisi

Ambari fornisce numerosi avvisi, che possono avere uno degli stati seguenti:

* **OK**
* **Warning**
* **CRITICAL**
* **UNKNOWN**

Gli avvisi con stato diverso da **OK** determinano la compilazione del campo **# alerts** (N. avvisi) nella parte superiore della pagina con il numero di avvisi. Selezionando questa opzione vengono visualizzati gli avvisi e il relativo stato.

Gli avvisi sono organizzati in diversi gruppi predefiniti, che possono essere visualizzati dalla pagina **Alerts** .

![pagina degli avvisi](./media/hdinsight-hadoop-manage-ambari/alerts.png)

È possibile gestire i gruppi scegliendo **Manage Alert Groups** (Gestisci gruppi di avvisi) dal menu **Actions** (Azioni). In questo modo è possibile modificare i gruppi esistenti o crearne dei nuovi.

![finestra di dialogo Manage Alert Groups](./media/hdinsight-hadoop-manage-ambari/manage-alerts.png)

È anche possibile gestire metodi di avviso e creare notifiche di avviso dal menu **Azioni**, selezionando __Manage Alert Notifications__ (Gestire notifiche di avviso). L'opzione consente di visualizzare le notifiche correnti e di creare nuove notifiche. È possibile inviare notifiche tramite **EMAIL** o **SNMP** al verificarsi di specifiche combinazioni di avviso/gravità. Ad esempio, è possibile inviare un avviso quando uno degli avvisi nel gruppo **YARN Default** (Impostazioni predefinite Yarn) è impostato su **Critical** (Critico).

![finestra di dialogo Create Alert](./media/hdinsight-hadoop-manage-ambari/create-alert-notification.png)

Infine, selezionare __Manage Alert Settings__ (Gestire impostazioni di avviso) dal menu __Azioni__ per stabilire il numero di volte in cui deve verificarsi un avviso prima che venga inviata una notifica. L'opzione può essere usata per evitare le notifiche di errori temporanei, ad esempio quando un servizio ha esito negativo su un solo nodo head e viene riavviato nell'altro nodo head.

### <a name="cluster"></a>HDInsight

La scheda **Metrics** del dashboard contiene una serie di widget che consentono di monitorare lo stato del cluster in modo immediato. Widget diversi, ad esempio **CPU Usage**, forniscono informazioni aggiuntive quando vengono selezionati.

![dashboard con metriche](./media/hdinsight-hadoop-manage-ambari/metrics.png)

La scheda **Heatmaps** visualizza le metriche come mappe termiche colorate, dal verde al rosso.

![dashboard con mappe termiche](./media/hdinsight-hadoop-manage-ambari/heatmap.png)

Per altre informazioni sui nodi del cluster, selezionare **Hosts**, quindi il nodo specifico desiderato.

![dettagli dell'host](./media/hdinsight-hadoop-manage-ambari/host-details.png)

### <a name="services"></a>Services

La barra laterale **Services** sul dashboard fornisce informazioni rapide sullo stato dei servizi in esecuzione nel cluster. Vengono usate diverse icone per indicare lo stato o le azioni da intraprendere, ad esempio un simbolo giallo di riciclo indica che un servizio deve essere riciclato.

![barra laterale dei servizi](./media/hdinsight-hadoop-manage-ambari/service-bar.png)

> [!NOTE]
> I servizi visualizzati sono diversi per le diverse versioni e tipi di cluster HDInsight. Quelli qui visualizzati possono differire da quelli visualizzati per il cluster.

Selezionando un servizio vengono visualizzate informazioni più dettagliate su di esso.

![informazioni di riepilogo sul servizio](./media/hdinsight-hadoop-manage-ambari/service-details.png)

#### <a name="quick-links"></a>Collegamenti rapidi

Alcuni servizi visualizzano un collegamento **Quick Links** nella parte superiore della pagina. Questi possono essere usati per accedere alle interfacce utente Web specifiche del servizio, ad esempio:

* **Job History** : cronologia dei processi MapReduce.
* **Resource Manager** : interfaccia utente di gestione risorse di YARN.
* **NameNode** : interfaccia utente NameNode del file system distribuito Hadoop (HDFS).
* **Oozie Web UI** : interfaccia utente Oozie.

Selezionando uno di questi collegamenti, verrà aperta una nuova scheda nel browser che visualizzerà la pagina selezionata.

> [!NOTE]
> Se si seleziona un **collegamento rapido** per un servizio, verrà visualizzato un errore di "server non trovato" a meno che non si usi un tunnel SSL (Secure Sockets Layer) per inoltrare il traffico Web al cluster. Questo comportamento è dovuto al fatto che le applicazioni Web usate per visualizzare queste informazioni non sono esposte in Internet.
>
> Per informazioni sull'uso di un tunnel SSL con HDInsight, vedere [Usare il tunneling SSH per accedere all'interfaccia Web di Ambari, ResourceManager, JobHistory, NameNode, Oozie e altre interfacce Web](hdinsight-linux-ambari-ssh-tunnel.md)

## <a name="management"></a>gestione

### <a name="ambari-users-groups-and-permissions"></a>Utenti, gruppi e autorizzazioni Ambari

L'uso di utenti, gruppi e autorizzazioni è supportato con un cluster HDInsight [aggiunto al dominio](hdinsight-domain-joined-introduction.md). Per informazioni sull'uso dell'interfaccia utente di gestione di Ambari in un cluster aggiunto al dominio, vedere [Gestire cluster HDInsight aggiunti al dominio](hdinsight-domain-joined-introduction.md).

> [!WARNING]
> Non modificare la password del watchdog Ambari (hdinsightwatchdog) nel cluster HDInsight basato su Linux. Se si modifica la password, non sarà più possibile usare azioni script o eseguire operazioni di ridimensionamento con il cluster.

### <a name="hosts"></a>Hosts

La pagina **Hosts** elenca tutti gli host del cluster. Per gestire gli host, seguire questa procedura.

![pagina Hosts](./media/hdinsight-hadoop-manage-ambari/hosts.png)

> [!NOTE]
> Non eseguire l'aggiunta, la rimozione o il ripristino di un host con i cluster HDInsight.

1. Selezionare uno o più host che si desidera gestire.

2. Usare il menu **Actions** per selezionare l'azione da eseguire:

   * **Start all components** : avvia tutti i componenti nell'host.

   * **Stop all components** : arresta tutti i componenti nell'host.

   * **Restart all components** : arresta e avvia tutti i componenti nell'host.

   * **Turn on maintenance mode** : elimina gli avvisi per l'host. Questa opzione deve essere abilitata se si eseguono azioni che genereranno avvisi, ad esempio il riavvio di un servizio da cui dipendono i servizi in esecuzione.

   * **Turn off maintenance mode** : ripristina la gestione normale degli avvisi dell'host.

   * **Stop** : arresta DataNode o NodeManagers nell'host.

   * **Start** : avvia DataNode o NodeManagers nell'host.

   * **Restart** : arresta e avvia DataNode o NodeManagers nell'host.

   * **Decommission** : rimuove un host dal cluster.

     > [!NOTE]
     > Non usare questa azione nei cluster HDInsight.

   * **Recommission** : aggiunge al cluster un host precedentemente rimosso.

     > [!NOTE]
     > Non usare questa azione nei cluster HDInsight.

### <a id="service"></a>Services

Nella pagina **Dashboard** (Dashboard) o **Services** (Servizi) usare il pulsante **Actions** (Azioni) nella parte inferiore dell'elenco di servizi per arrestare e avviare tutti i servizi.

![Service Actions](./media/hdinsight-hadoop-manage-ambari/service-actions.png)

> [!WARNING]
> Anche se **Add Service** (Aggiungi servizio) è elencato in questo menu, non deve essere usato per aggiungere servizi al cluster HDInsight. È necessario aggiungere nuovi servizi.utilizzando un'azione di Script durante il provisioning del cluster. Per altre informazioni su come utilizzare le azioni di script, vedere [Personalizzare cluster HDInsight mediante l'azione script](hdinsight-hadoop-customize-cluster-linux.md).

Il pulsante **Actions** consente di riavviare tutti i servizi, ma spesso si desidera avviare, arrestare o riavviare un servizio specifico. Seguire questa procedura per effettuare azioni per un singolo servizio:

1. Nella pagina **Dashboard** (Dashboard) o **Services** (Servizi) selezionare un servizio.

2. Nella parte superiore della scheda **Summary** (Riepilogo) fare clic sul pulsante **Service Actions** (Azioni servizio) e selezionare l'azione da intraprendere. Il servizio viene riavviato in tutti i nodi.

    ![azione del servizio](./media/hdinsight-hadoop-manage-ambari/individual-service-actions.png)

   > [!NOTE]
   > Se si riavviano alcuni servizi mentre il cluster è in esecuzione, è possibile che vengano generati avvisi. Per evitare questo problema, è possibile abilitare la modalità di manutenzione facendo clic sul pulsante **Service Actions** (Azioni servizio) per abilitare la **modalità manutenzione** per il servizio prima di eseguire il riavvio.

3. Dopo aver selezionato un'azione, il valore nel campo **# op** nella parte superiore della pagina aumenta per indicare che è in corso un'operazione in background. Se configurato, viene visualizzato l'elenco delle operazioni in background.

   > [!NOTE]
   > Se è stata abilitata la **modalità di manutenzione** per il servizio, ricordarsi di disabilitarla usando il pulsante **Service Actions** (Azioni servizio) al termine dell'operazione.

Per configurare un servizio, seguire questa procedura:

1. Nella pagina **Dashboard** (Dashboard) o **Services** (Servizi) selezionare un servizio.

2. Selezionare la scheda **Configs** (Configurazioni). Verrà visualizzata la configurazione corrente, insieme a un elenco delle configurazioni precedenti.

    ![configurazioni](./media/hdinsight-hadoop-manage-ambari/service-configs.png)

3. Usare i campi visualizzati per modificare la configurazione, quindi selezionare **Save**. In alternativa, selezionare una configurazione precedente, quindi **Make current** per ripristinare le impostazioni precedenti.

## <a name="ambari-views"></a>Viste di Ambari

Le viste di Ambari consentono agli sviluppatori di collegare gli elementi dell'interfaccia utente all'interfaccia utente Web di Ambari usando il [framework delle viste di Ambari](https://cwiki.apache.org/confluence/display/AMBARI/Views). HDInsight fornisce le viste seguenti i con tipi di cluster Hadoop:

* Gestore di code Yarn: il gestore delle code fornisce un'interfaccia utente semplice per la visualizzazione e la modifica di code YARN.

* Viste di Hive: le viste di Hive consentono di eseguire query Hive direttamente dal Web browser. È possibile salvare query, visualizzare i risultati, salvare i risultati nell'archiviazione cluster o scaricare i risultati nel sistema locale. Per altre informazioni sull'uso delle viste di Hive, vedere l'argomento relativo all' [uso delle viste Hive con HDInsight](hdinsight-hadoop-use-hive-ambari-view.md).

* Vista di Tez: la vista di Tez consente di comprendere e ottimizzare i processi visualizzando le informazioni sull'esecuzione dei processi di Tez e quali risorse sono usate dal processo.

