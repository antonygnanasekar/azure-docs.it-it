---
title: Introduzione a HBase in Azure HDInsight | Documentazione Microsoft
description: Seguire questa esercitazione di HBase per iniziare a usare Apache HBase con Hadoop in HDInsight. Creare tabelle dalla shell HBase e sottoporle a query tramite Hive.
keywords: Apache hbase, hbase, shell di hbase, esercitazione hbase
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 4d6a2658-6b19-4268-95ee-822890f5a33a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/22/2017
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 21d8dff230e045607b70013f4eabf1bfe8ec3993
ms.lasthandoff: 03/25/2017


---
# <a name="hbase-tutorial-get-started-using-apache-hbase-in-hdinsight"></a>Esercitazione di HBase: Introduzione all'uso di Apache HBase in HDInsight

Informazioni su come creare un cluster HBase in HDInsight, creare tabelle HBase ed eseguire query sulle tabelle con Hive. Per informazioni generali su HBase, vedere [Panoramica di HDInsight HBase][hdinsight-hbase-overview].

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="prerequisites"></a>Prerequisiti
Prima di iniziare questa esercitazione di HBase, è necessario disporre di quanto segue:

* **Una sottoscrizione di Azure**. Vedere [Ottenere una versione di valutazione gratuita di Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* [Secure Shell (SSH)](hdinsight-hadoop-linux-use-ssh-unix.md). 
* [curl](http://curl.haxx.se/download.html).

### <a name="access-control-requirements"></a>Requisiti di controllo di accesso
[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-hbase-cluster"></a>Nome del cluster HBase
La procedura seguente usa un modello di Azure Resource Manager per creare un cluster HBase basato su Linux versione 3.4 e il valore predefinito dipendente dall'account di archiviazione di Azure. Per comprendere i parametri usati nella procedure e altri metodi di creazione del cluster, vedere [Creare cluster Hadoop basati su Linux in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Fare clic sull'immagine seguente per aprire il modello nel portale di Azure. Il modello è disponibile in un contenitore BLOB pubblico. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-cluster-in-hdinsight.json" target="_blank"><img src="./media/hdinsight-hbase-tutorial-get-started-linux/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. Compilare i campi seguenti del pannello **Distribuzione personalizzata**:
   
   * **Sottoscrizione**: consente di selezionare la sottoscrizione di Azure che verrà usata per creare il cluster.
   * **Gruppo di risorse**: consente di creare un nuovo gruppo di Azure Resource Manager o di selezionarne uno esistente.
   * **Posizione**: consente di specificare la posizione del gruppo di risorse. 
   * **ClusterName**(NomeCluster): immettere un nome per il cluster HBase che verrà creato.
   * **Cluster login name and password**: il nome dell'account di accesso predefinito è **admin**.
   * **SSH username and password**: il nome utente predefinito è **sshuser**.  È possibile rinominarlo.
     
     Gli altri parametri sono facoltativi.  
     
     Ogni cluster ha una dipendenza dall'account di Archiviazione di Azure. Dopo aver eliminato un cluster, i dati vengono mantenuti nell'account di archiviazione. Il nome dell'account di archiviazione predefinito del cluster è il nome del cluster a cui viene aggiunto "store". È hardcoded nella sezione delle variabili del modello.
3. Selezionare **Accetto le condizioni riportate sopra**, quindi fare clic su **Acquista**. La creazione di un cluster richiede circa 20 minuti.

> [!NOTE]
> Dopo l'eliminazione di un cluster HBase, è possibile creare un altro cluster HBase usando lo stesso contenitore di BLOB predefinito. Il nuovo cluster selezionerà le tabelle HBase create nel cluster originale. Per evitare incoerenze, è consigliabile disabilitare le tabelle HBase prima di eliminare il cluster.
> 
> 

## <a name="create-tables-and-insert-data"></a>Creare tabelle e inserire dati
È possibile usare SSH per connettersi ai cluster HBase e usare la shell di HBase per creare tabelle HBase, inserire dati ed eseguire query sui dati. Per altre informazioni, vedere [Usare SSH con HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

Per la maggior parte delle persone, i dati vengono visualizzati in formato tabulare:

![Tabella con dati HBase di HDInsight][img-hbase-sample-data-tabular]

In HBase, che rappresenta un'implementazione di BigTable, gli stessi dati sono simili a:

![Dati BigTable HBase di HDInsight][img-hbase-sample-data-bigtable]

Ciò può essere più utile dopo avere completato la procedura successiva.  

**Per usare la shell HBase**

1. In SSH eseguire il comando seguente:
   
        hbase shell
2. Creare un HBase con famiglie di due colonne:
   
        create 'Contacts', 'Personal', 'Office'
        list
3. Inserire alcuni dati:
   
        put 'Contacts', '1000', 'Personal:Name', 'John Dole'
        put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
        put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
        put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
        scan 'Contacts'
   
    ![Shell HBase Hadoop di HDInsight][img-hbase-shell]
4. Ottenere una singola riga
   
        get 'Contacts', '1000'
   
    Verranno visualizzati gli stessi risultati usando il comando di analisi, poiché esiste solo una riga.
   
    Per altre informazioni sullo schema di tabella HBase, vedere [Introduzione alla progettazione dello schema HBase][hbase-schema]. Per altri comandi HBase, vedere la [guida di riferimento di Apache HBase][hbase-quick-start].
5. Chiudere la shell
   
        exit

**Per il caricamento bulk dei dati nella tabella HBase dei contatti**

HBase include diversi metodi di caricamento dei dati nelle tabelle.  Per altre informazioni, vedere [Caricamento bulk](http://hbase.apache.org/book.html#arch.bulk.load).

Un file di dati di esempio è stato caricato in un contenitore BLOB pubblico, *wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt*.  Il contenuto del file di dati è il seguente:

    8396    Calvin Raji      230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu         646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie         508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson     674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller    397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile     592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee        870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes       599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander  670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander    998-555-0171    230-555-0200    771 Northridge Drive

È possibile creare un file di testo e caricare il file nel proprio account di archiviazione. Per le istruzioni, vedere [Caricare dati per processi Hadoop in HDInsight][hdinsight-upload-data].

> [!NOTE]
> In questa procedura viene utilizzata la tabella HBase dei contatti creata nella procedura precedente.
> 
> 

1. In SSH eseguire il comando seguente per trasformare il file di dati in StoreFiles e archiviarlo in un percorso relativo specificato da Dimporttsv.bulk.output:  Se si è nella shell di HBase, usare il comando exit per uscire.
   
        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name,Personal:Phone,Office:Phone,Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasbs://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt
2. Eseguire questo comando per caricare i dati da /example/data/storeDataFileOutput nella tabella di HBase:
   
        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts
3. È possibile aprire la shell HBase e usare il comando di analisi per visualizzare il contenuto della tabella.

## <a name="use-hive-to-query-hbase"></a>Usare Hive per eseguire query su HBase
È possibile eseguire query sui dati nelle tabelle HBase tramite Hive. In questa sezione una tabella Hive mappata alla tabella HBase viene creata e usata per interrogare i dati nella tabella HBase.

> [!NOTE]
> Se Hive e HBase si trovano su cluster diversi nella stessa rete virtuale, è necessario superare il quorum di zookeeper durante la chiamata alla shell di Hive:
>
>       hive --hiveconf hbase.zookeeper.quorum=zk0-xxxx.xxxxxxxxxxxxxxxxxxxxxxx.cx.internal.cloudapp.net,zk1-xxxx.xxxxxxxxxxxxxxxxxxxxxxx.cx.internal.cloudapp.net,zk2-xxxx.xxxxxxxxxxxxxxxxxxxxxxx.cx.internal.cloudapp.net --hiveconf zookeeper.znode.parent=/hbase-unsecure  
>
>

1. Aprire **PuTTY**e connettersi al cluster.  Vedere le istruzioni nella procedura precedente.
2. Aprire la shell di Hive.
   
       hive
       
3. Eseguire il seguente script HiveQL per creare una tabella Hive mappata alla tabella HBase. Prima di eseguire questa istruzione, assicurarsi di aver creato la tabella di esempio usata precedentemente in questa esercitazione come riferimento con la shell HBase.
   
        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');
4. Eseguire il seguente script HiveQL per eseguire query sui dati nella tabella HBase:
   
         SELECT count(*) FROM hbasecontacts;

## <a name="use-hbase-rest-apis-using-curl"></a>Usare le API REST HBase mediante Curl
> [!NOTE]
> Quando si usa Curl o qualsiasi altra forma di comunicazione REST con WebHCat, è necessario autenticare le richieste fornendo il nome utente e la password dell'amministratore cluster HDInsight. È inoltre necessario specificare il nome del cluster come parte dell'URI (Uniform Resource Identifier) usato per inviare le richieste al server.
> 
> Per i comandi riportati in questa sezione, sostituire **USERNAME** con l'utente da autenticare nel cluster e **PASSWORD** con la password dell'account utente. Sostituire **CLUSTERNAME** con il nome del cluster.
> 
> L'API REST viene protetta tramite l' [autenticazione di base](http://en.wikipedia.org/wiki/Basic_access_authentication). È necessario effettuare sempre le richieste usando il protocollo HTTPS (Secure HTTP) per essere certi che le credenziali vengano inviate in modo sicuro al server.
> 
> 

1. Da una riga di comando usare il comando seguente per verificare che sia possibile connettersi al cluster HDInsight:
   
        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/templeton/v1/status
   
    Dovrebbe essere visualizzato un messaggio simile al seguente:
   
        {"status":"ok","version":"v1"}
   
    I parametri usati in questo comando sono i seguenti:
   
   * **-u** : il nome utente e la password usati per autenticare la richiesta.
   * **-G** : indica che si tratta di una richiesta GET.
2. Usare il comando seguente per ottenere l'elenco delle tabelle HBase esistenti:
   
        curl -u <UserName>:<Password> \
        -G https://<ClusterName>.azurehdinsight.net/hbaserest/
3. Usare il comando seguente per creare una nuova tabella HBase con famiglie di due colonne:
   
        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/schema" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"@name\":\"Contact1\",\"ColumnSchema\":[{\"name\":\"Personal\"},{\"name\":\"Office\"}]}" \
        -v
   
    Lo schema viene fornito nel formato JSON.
4. Usare il comando seguente per inserire alcuni dati:
   
        curl -u <UserName>:<Password> \
        -X PUT "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/false-row-key" \
        -H "Accept: application/json" \
        -H "Content-Type: application/json" \
        -d "{\"Row\":[{\"key\":\"MTAwMA==\",\"Cell\": [{\"column\":\"UGVyc29uYWw6TmFtZQ==\", \"$\":\"Sm9obiBEb2xl\"}]}]}" \
        -v
   
    È necessario applicare la codifica base64 ai valori specificati nell'interruttore -d.  Nell'esempio:
   
   * MTAwMA==: 1000
   * UGVyc29uYWw6TmFtZQ==: Personal:Name
   * Sm9obiBEb2xl: John Dole
     
     [false-row-key](https://hbase.apache.org/apidocs/org/apache/hadoop/hbase/rest/package-summary.html#operation_cell_store_single) consente di inserire più valori in batch.
5. Usare il comando seguente per ottenere una riga:
   
        curl -u <UserName>:<Password> \
        -X GET "https://<ClusterName>.azurehdinsight.net/hbaserest/Contacts1/1000" \
        -H "Accept: application/json" \
        -v

Per altre informazioni sulle API REST HBase, vedere la [Apache HBase Reference Guide](https://hbase.apache.org/book.html#_rest)(Guida di riferimento di Apache HBase).

>
> [!NOTE]
> Thrift non è supportato da HBase in HDInsight.
>

## <a name="check-cluster-status"></a>Controllare lo stato del cluster
HBase in HDInsight viene fornito con un'interfaccia utente Web per il monitoraggio dei cluster. Usando l’interfaccia Web è possibile richiedere statistiche o informazioni sulle aree.

**Per accedere all'interfaccia utente master HBase**

1. Aprire l'interfaccia utente Web Ambari all'indirizzo https://&lt;Nomecluster>.azurehdinsight.net.
2. Fare clic su **HBase** nel menu di sinistra.
3. Fare clic su **Quick links** (Collegamenti rapidi) nella parte superiore della pagina, scegliere il collegamento di nodo Zookeeper attivo e quindi fare clic su **HBase Master UI** (Interfaccia utente master HBase).  L'interfaccia utente viene aperta in un'altra scheda del browser:

  ![Interfaccia utente master HBase di HDInsight](./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hmaster-ui.png)

  L'interfaccia utente master HBase contiene le sezioni seguenti:

  - server di zona
  - master di backup
  - tables
  - attività
  - attributi di software

## <a name="delete-the-cluster"></a>Eliminazione del cluster
Per evitare incoerenze, è consigliabile disabilitare le tabelle HBase prima di eliminare il cluster.

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Passaggi successivi
In questa esercitazione di HBase per HDInsight si è appreso come creare tabelle e un cluster HBase e come visualizzare i dati delle tabelle dalla shell HBase. Si è inoltre appreso come usare una query Hive sui dati nelle tabelle HBase e come usare le API REST C# di HBase per creare una tabella HBase e recuperare i dati dalla tabella.

Per altre informazioni, vedere:

* [Panoramica di HDInsight HBase][hdinsight-hbase-overview]: HBase è un database NoSQL open source Apache basato su Hadoop che fornisce un accesso casuale e coerenza assoluta a grandi quantità di dati non strutturati e semistrutturati.

[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hbase-reference]: http://hbase.apache.org/book.html#importtsv
[hbase-schema]: http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf
[hbase-quick-start]: http://hbase.apache.org/book.html#quickstart





[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-versions]: hdinsight-component-versioning.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-portal]: https://portal.azure.com/
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/

[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-quick-create.png
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-hive-editor.png
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-file-browser.png
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-shell.png
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-tabular.png
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started-linux/hdinsight-hbase-contacts-bigtable.png

