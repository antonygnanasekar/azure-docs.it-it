---
title: Usare i notebook di Zeppelin con cluster Apache Spark in Azure HDInsight | Documentazione Microsoft
description: Istruzioni dettagliate su come usare i notebook di Zeppelin con cluster Apache Spark in Azure HDInsight.
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: df489d70-7788-4efa-a089-e5e5006421e2
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: a939a0845d7577185ff32edd542bcb2082543a26
ms.openlocfilehash: a5494f16e3398be507080dd4fac591144f69d9fc
ms.lasthandoff: 01/24/2017


---
# <a name="use-zeppelin-notebooks-with-apache-spark-cluster-on-azure-hdinsight"></a>Usare i notebook di Zeppelin con cluster Apache Spark in Azure HDInsight

I cluster HDInsight Spark includono notebook Zeppelin che possono essere usati per eseguire processi Spark. Questo articolo illustra come usare il notebook Zeppelin in un cluster HDInsight.

> [!NOTE]
> Per impostazione predefinita, i notebook di Zeppelin sono disponibili solo per Spark 1.6.2 in cluster HDInsight versione 3.5. Se si vuole usare Zeppelin in altre versioni di cluster HDInsight Spark, è possibile installare Zeppelin usando l'azione script. Per istruzioni, vedere [Installare notebook Zeppelin per cluster Apache Spark in HDInsight Linux](hdinsight-apache-spark-use-zeppelin-notebook.md).
> 
>

**Prerequisiti:**

* Una sottoscrizione di Azure. Vedere [Ottenere una versione di valutazione gratuita di Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Un cluster Apache Spark in HDInsight. Per istruzioni, vedere l'articolo relativo alla [creazione di cluster Apache Spark in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="launch-a-zeppelin-notebook"></a>Avviare un notebook Zeppelin
1. Nel pannello del cluster Spark fare clic su **Dashboard cluster** e quindi su **Notebook di Zeppelin**. Se richiesto, immettere le credenziali per il cluster.
   
   > [!NOTE]
   > È anche possibile raggiungere il notebook di Zeppelin per il cluster aprendo l'URL seguente nel browser. Sostituire **CLUSTERNAME** con il nome del cluster:
   > 
   > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`
   > 
   > 
2. Creare un nuovo notebook. Dal riquadro intestazione fare clic su **Notebook** e quindi su **Create New Note** (Crea una nuova nota).
   
    ![Creare un nuovo notebook Zeppelin](./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.createnewnote.png "Creare un nuovo notebook Zeppelin")
   
    Immettere un nome per il notebook e quindi fare clic su **Create New Note** (Crea una nuova nota).
3. Verificare anche che l'intestazione del notebook mostri uno stato connesso, indicato da un punto verde nell'angolo superiore destro.
   
    ![Stato del notebook Zeppelin](./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.newnote.connected.png "Stato del notebook Zeppelin")
4. Caricare i dati di esempio in una tabella temporanea. Quando si crea un cluster Spark in HDInsight, il file di dati di esempio, **hvac.csv**, viene copiato nell'account di archiviazione associato in **\HdiSamples\SensorSampleData\hvac**.
   
    Nel paragrafo vuoto creato per impostazione predefinita del nuovo notebook, incollare il frammento di codice riportato di seguito.
   
        %livy.spark
        //The above magic instructs Zeppelin to use the Livy Scala interpreter
   
        // Create an RDD using the default Spark context, sc
        val hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
        // Define a schema
        case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)
   
        // Map the values in the .csv file to the schema
        val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
            s => Hvac(s(0), 
                    s(1),
                    s(2).toInt,
                    s(3).toInt,
                    s(6)
            )
        ).toDF()
   
        // Register as a temporary table called "hvac"
        hvac.registerTempTable("hvac")
   
    Premere **MAIUSC+INVIO** oppure fare clic sul pulsante **Play** (Riproduci) in modo che il paragrafo esegua il frammento di codice. Lo stato nell'angolo destro del paragrafo deve passare da PRONTO, IN ATTESA, IN ESECUZIONE, a COMPLETATO. L'output viene visualizzato nella parte inferiore dello stesso paragrafo. Nella schermata è simile al seguente:
   
    ![Creare una tabella temporanea dai dati non elaborati](./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.loaddDataintotable.png "Creare una tabella temporanea dai dati non elaborati")
   
    È inoltre possibile fornire un titolo a ogni paragrafo. Nell'angolo superiore destro fare clic sull'icona **Settings** (Impostazioni) e quindi su **Show title** (Mostra titolo).
5. È ora possibile eseguire istruzioni SQL Spark su tabella **hvac** . Incollare la query seguente in un nuovo paragrafo. La query recupera l'ID dell'edificio e la differenza tra le temperature effettive e quelle di destinazione per ogni edificio in una determinata data. Premere **MAIUSC + INVIO**.
   
        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13" 
   
    L'istruzione **%sql** all'inizio indica al notebook di usare l'interprete Livy Scala.
   
    Nella schermata riportata di seguito sono illustrate questo output.
   
    ![Eseguire un'istruzione SQL Spark usando il notebook](./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.sparksqlquery1.png "Eseguire un'istruzione SQL Spark usando il notebook")
   
     Scegliere le opzioni di visualizzazione (evidenziate nel rettangolo) per passare tra diverse rappresentazioni per lo stesso output. Fare clic su **Impostazioni** per scegliere la chiave e i valori nell'output. La schermata precedente usa **buildingID** come chiave e la media di **temp_diff** come valore.
6. È inoltre possibile eseguire istruzioni SQL Spark tramite le variabili nella query. Il seguente frammento di codice illustra come definire una variabile **Temp**nella query con i valori possibili che si vuole eseguire. Quando si esegue la query per la prima volta, un elenco a tendina viene popolato automaticamente con i valori specificati per la variabile.
   
        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}" 
   
    Incollare questo frammento di codice in un nuovo paragrafo e premere **MAIUSC+INVIO**. Nella schermata riportata di seguito sono illustrate questo output.
   
    ![Eseguire un'istruzione SQL Spark usando il notebook](./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.sparksqlquery2.png "Eseguire un'istruzione SQL Spark usando il notebook")
   
    Per le query successive, è possibile selezionare un nuovo valore dall'elenco a tendina e quindi eseguire nuovamente la query. Fare clic su **Impostazioni** per scegliere la chiave e i valori nell'output. La schermata precedente usa **buildingID** come chiave, la media di **temp_diff** come valore e **targettemp** come gruppo.
7. Riavviare l'interprete Livy per uscire dall'applicazione. A tale scopo, aprire le impostazioni dell'interprete facendo clic sul nome dell'utente connesso nell'angolo superiore destro e quindi fare clic su **Interpreter** (Interprete).
   
    ![Avviare l'interprete](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Output Hive")
8. Scorrere fino alle impostazioni dell'interprete e quindi fare clic su **Restart** (Riavvia).
   
    ![Riavviare l'interprete Livy](./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.zeppelin.restart.interpreter.png "Riavviare l'interprete Zeppelin")

## <a name="how-do-i-use-external-packages-with-the-notebook"></a>Come usare pacchetti esterni con il notebook
È possibile configurare il notebook Zeppelin in un cluster Apache Spark in HDInsight (Linux) per l'uso di pacchetti esterni creati dalla community che non sono inclusi come predefiniti nel cluster. Per un elenco completo dei pacchetti disponibili, è possibile eseguire ricerche nel [repository Maven](http://search.maven.org/) . È anche possibile ottenere un elenco dei pacchetti disponibili da altre origini. Ad esempio, un elenco completo dei pacchetti creati dalla community è disponibile nel sito Web [spark-packages.org](http://spark-packages.org/).

Questo articolo illustra come usare il pacchetto [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) con il notebook Jupyter.

1. Aprire le impostazioni dell'interprete. Nell'angolo superiore destro fare clic sul nome dell'utente connesso e quindi su **Interpreter** (Interprete).
   
    ![Avviare l'interprete](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Output Hive")
2. Scorrere fino alle impostazioni dell'interprete e quindi fare clic su **Edit** (Modifica).
   
    ![Modificare le impostazioni dell'interprete](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "Modificare le impostazioni dell'interprete")
3. Aggiungere una nuova chiave denominata **livy.spark.jars.packages** e impostarne il valore nel formato `group:id:version`. Se si vuole usare il pacchetto [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar), è quindi necessario impostare il valore della chiave su `com.databricks:spark-csv_2.10:1.4.0`.
   
    ![Modificare le impostazioni dell'interprete](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "Modificare le impostazioni dell'interprete")
   
    Fare clic su **Save** (Salva) e quindi riavviare l'interprete Livy.
4. **Suggerimento**: il valore della chiave immesso sopra si determina come illustrato di seguito.
   
    a. Individuare un pacchetto nel repository Maven. In questa esercitazione è stato usato [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
   
    b. Recuperare dal repository i valori per **GroupId**, **ArtifactId** e **Version**.
   
    ![Usare pacchetti esterni con notebook di Jupyter](./media/hdinsight-apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "Usare pacchetti esterni con notebook di Jupyter")
   
    c. Concatenare i tre valori, separati da due punti (**:**).
   
        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-the-zeppelin-notebooks-saved"></a>Posizione di salvataggio dei notebook Zeppelin
I notebook Zeppelin vengono salvati nei nodi head del cluster. Se si elimina il cluster, verranno quindi eliminati anche i notebook. Se si vogliono mantenere i notebook per usarli successivamente in altri cluster, è necessario esportarli al termine dell'esecuzione dei processi. Per esportare un notebook, fare clic sull'icona di **esportazione** come illustrato nell'immagine seguente.

![Scaricare notebook](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "Scaricare il notebook")

Il notebook verrà così salvato come file JSON nel percorso di download dell'utente.

## <a name="livy-session-management"></a>Gestione delle sessioni di Livy
Quando si esegue il primo paragrafo di codice nel notebook Zeppelin, nel cluster HDInsight Spark viene creata una nuova sessione di Livy. Tale sessione sarà condivisa da tutti i notebook Zeppelin creati successivamente. Se la sessione di Livy viene terminata per qualche motivo (riavvio del cluster e così via), non sarà possibile eseguire processi dal notebook Zeppelin.

In tal caso, per poter eseguire processi dal notebook Zeppelin è prima necessario seguire questa procedura. 

1. Riavviare l'interprete Livy dal notebook Zeppelin. A tale scopo, aprire le impostazioni dell'interprete facendo clic sul nome dell'utente connesso nell'angolo superiore destro e quindi fare clic su **Interpreter** (Interprete).
   
    ![Avviare l'interprete](./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Output Hive")
2. Scorrere fino alle impostazioni dell'interprete e quindi fare clic su **Restart** (Riavvia).
   
    ![Riavviare l'interprete Livy](./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.zeppelin.restart.interpreter.png "Riavviare l'interprete Zeppelin")
3. Eseguire una cella di codice da un notebook Zeppelin esistente. Verrà così creata una nuova sessione di Livy nel cluster HDInsight.

## <a name="seealso"></a>Vedere anche
* [Panoramica: Apache Spark su Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenari
* [Spark con Business Intelligence: eseguire l'analisi interattiva dei dati con strumenti di Business Intelligence mediante Spark in HDInsight](hdinsight-apache-spark-use-bi-tools.md)
* [Spark con Machine Learning: utilizzare Spark in HDInsight per l'analisi della temperatura di compilazione utilizzando dati HVAC](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark con Machine Learning: usare Spark in HDInsight per prevedere i risultati del controllo degli alimenti](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Streaming Spark: usare Spark in HDInsight per la creazione di applicazioni di streaming in tempo reale](hdinsight-apache-spark-eventhub-streaming.md)
* [Analisi dei log del sito Web mediante Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Creare ed eseguire applicazioni
* [Creare un'applicazione autonoma con Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Eseguire processi in modalità remota in un cluster Spark usando Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Strumenti ed estensioni
* [Usare il plug-in degli strumenti HDInsight per IntelliJ IDEA per creare e inviare applicazioni Spark in Scala](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Utilizzare il plug-in Strumenti HDInsight per IntelliJ IDEA per eseguire il debug di applicazioni Spark in remoto](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Kernel disponibili per notebook di Jupyter nel cluster Spark per HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Usare pacchetti esterni con i notebook Jupyter](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installare Jupyter Notebook nel computer e connetterlo a un cluster HDInsight Spark](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Gestire risorse
* [Gestire le risorse del cluster Apache Spark in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Tenere traccia ed eseguire il debug di processi in esecuzione nel cluster Apache Spark in Azure HDInsight](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md 








