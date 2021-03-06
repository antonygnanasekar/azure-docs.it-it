---
title: 'Esercitazione su NoSQL: Azure DocumentDB Java SDK | Microsoft Docs'
description: "Esercitazione su NoSQL che crea un database online e un&quot;applicazione console Java con DocumentDB Java SDK. Azure DocumentDB è un database NoSQL per JSON."
keywords: esercitazione su nosql, database online, applicazione console java
services: documentdb
documentationcenter: Java
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: 75a9efa1-7edd-4fed-9882-c0177274cbb2
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 01/05/2017
ms.author: arramac
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: da7907ffc515ea2e3040075c93bcd53840cf3ff5
ms.lasthandoff: 03/29/2017


---
# <a name="nosql-tutorial-build-a-documentdb-java-console-application"></a>Esercitazione su NoSQL: Compilare un'applicazione console Java di DocumentDB
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Node.js per MongoDB](documentdb-mongodb-samples.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 

Esercitazione su NoSQL per Azure DocumentDB Java SDK Dopo aver seguito questa esercitazione, si otterrà un'applicazione console che consente di creare e ridefinire le query delle risorse DocumentDB.

Argomenti trattati:

* Creazione e connessione a un account DocumentDB
* Configurare una soluzione Visual Studio
* Creazione di un database online
* Creare una raccolta
* Creazione di documenti JSON
* Esecuzione di query sulla raccolta
* Creazione di documenti JSON
* Esecuzione di query sulla raccolta
* Sostituzione di un documento
* Eliminazione di un documento
* Eliminazione del database

Ecco come procedere.

## <a name="prerequisites"></a>Prerequisiti
Assicurarsi che sia disponibile quanto segue:

* Un account Azure attivo. Se non si ha un account, è possibile iscriversi per ottenere un [account gratuito](https://azure.microsoft.com/free/). In alternativa, per questa esercitazione è possibile usare l'[emulatore DocumentDB di Azure](documentdb-nosql-local-emulator.md).
* [Git](https://git-scm.com/downloads)
* [Java Development Kit (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
* [Maven](http://maven.apache.org/download.cgi).

## <a name="step-1-create-a-documentdb-account"></a>Passaggio 1: Creare un account DocumentDB
Creare un account DocumentDB. Se è già disponibile un account da usare, è possibile andare direttamente al passaggio [Clonare il progetto GitHub](#GitClone). Se si usa l'emulatore DocumentDB, seguire la procedura illustrata nell'articolo relativo all'[emulatore DocumentDB di Azure](documentdb-nosql-local-emulator.md) per configurare l'emulatore e proseguire con il passaggio [Clonare il progetto GitHub](#GitClone).

[!INCLUDE [documentdb-create-dbaccount](../../includes/documentdb-create-dbaccount.md)]

## <a id="GitClone"></a>Passaggio 2: Clonare il progetto GitHub
Per iniziare, è possibile clonare il repository GitHub per l'[introduzione a DocumentDB e Java](https://github.com/Azure-Samples/documentdb-java-getting-started). Eseguire questo comando da una directory locale per recuperare il progetto di esempio in locale.

    git clone git@github.com:Azure-Samples/documentdb-java-getting-started.git

    cd documentdb-java-getting-started

La directory contiene un oggetto `pom.xml` per il progetto e una cartella `src` che contiene il codice sorgente Java incluso `Program.java`, che mostra come eseguire semplici operazioni con Azure DocumentDB, come la creazione di documenti e l'esecuzione di query sui dati all'interno di una raccolta. L'oggetto `pom.xml` include una dipendenza per [DocumentDB Java SDK su Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb).

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-documentdb</artifactId>
        <version>LATEST</version>
    </dependency>

## <a id="Connect"></a>Passaggio 3: Connettersi a un account DocumentDB
Tornare al [portale di Azure](https://portal.azure.com) per recuperare l'endpoint e la chiave master primaria. La chiave primaria e l'endpoint di DocumentDB sono necessari all'applicazione per conoscere la destinazione della connessione e a DocumentDB per considerare attendibile la connessione dell'applicazione.

Nel portale di Azure passare all'account DocumentDB e quindi fare clic su **Chiavi**. Copiare l'URI dal portale e incollarlo in `<your endpoint URI>` nel file Program.java. Copiare quindi la CHIAVE PRIMARIA dal portale e incollarla in `<your key>`.

    this.client = new DocumentClient(
        "<your endpoint URI>",
        "<your key>"
        , new ConnectionPolicy(),
        ConsistencyLevel.Session);

![Screenshot del portale di Azure usato nell'esercitazione su NoSQL per creare un'applicazione console Java. Mostra un account DocumentDB, con l'hub ACTIVE evidenziato, il pulsante CHIAVI evidenziato nel pannello dell'account DocumentDB e i valori di URI, CHIAVE PRIMARIA e CHIAVE SECONDARIA evidenziati nel pannello Chiavi][keys]

## <a name="step-4-create-a-database"></a>Passaggio 4: Creare un database
È possibile creare un [database](documentdb-resources.md#databases) di DocumentDB usando il metodo [createDatabase](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#createDatabase-com.microsoft.azure.documentdb.Database-com.microsoft.azure.documentdb.RequestOptions-) della classe **DocumentClient**. Un database è il contenitore logico per l'archiviazione di documenti JSON partizionato nelle raccolte.

    Database database = new Database();
    database.setId("familydb");
    this.client.createDatabase(database, null);

## <a id="CreateColl"></a>Passaggio 5: Creare una raccolta
> [!WARNING]
> **createCollection** crea una nuova raccolta con velocità effettiva riservata, che presenta implicazioni in termini di prezzi. Per altre informazioni, visitare la [pagina relativa ai prezzi](https://azure.microsoft.com/pricing/details/documentdb/).
> 
> 

È possibile creare una [raccolta](documentdb-resources.md#collections) usando il metodo [createCollection](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#createCollection-java.lang.String-com.microsoft.azure.documentdb.DocumentCollection-com.microsoft.azure.documentdb.RequestOptions-) della classe **DocumentClient**. Una raccolta è un contenitore di documenti JSON e di logica dell'applicazione JavaScript associata.


    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId("familycoll");

    // DocumentDB collections can be reserved with throughput specified in request units/second. 
    // Here we create a collection with 400 RU/s.
    RequestOptions requestOptions = new RequestOptions();
    requestOptions.setOfferThroughput(400);

    this.client.createCollection("/dbs/familydb", collectionInfo, requestOptions);

## <a id="CreateDoc"></a>Passaggio 6: Creare documenti JSON
È possibile creare un [documento](documentdb-resources.md#documents) usando il metodo [createDocument](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#createDocument-java.lang.String-java.lang.Object-com.microsoft.azure.documentdb.RequestOptions-boolean-) della classe **DocumentClient**. I documenti sono contenuto JSON definito dall'utente (arbitrario). Ora è possibile inserire uno o più documenti. Se sono già disponibili dati da archiviare nel database, è possibile usare lo [strumento di migrazione dei dati](documentdb-import-data.md) di DocumentDB per importare i dati in un database.

    // Insert your Java objects as documents 
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen")

    // More initialization skipped for brevity. You can have nested references
    andersenFamily.setParents(new Parent[] { parent1, parent2 });
    andersenFamily.setDistrict("WA5");
    Address address = new Address();
    address.setCity("Seattle");
    address.setCounty("King");
    address.setState("WA");

    andersenFamily.setAddress(address);
    andersenFamily.setRegistered(true);

    this.client.createDocument("/dbs/familydb/colls/familycoll", family, new RequestOptions(), true);

![Diagramma che illustra la relazione gerarchica tra l'account, il database online, la raccolta e i documenti usati nell'esercitazione su NoSQL per creare un'applicazione console Java](./media/documentdb-get-started/nosql-tutorial-account-database.png)

## <a id="Query"></a>Passaggio 7: Eseguire query sulle risorse di DocumentDB
DocumentDB supporta [query](documentdb-sql-query.md) complesse sui documenti JSON archiviati in ogni raccolta.  Il codice di esempio seguente mostra come eseguire query su documenti in DocumentDB usando la sintassi SQL con il metodo [queryDocuments](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#queryDocuments-java.lang.String-com.microsoft.azure.documentdb.SqlQuerySpec-com.microsoft.azure.documentdb.FeedOptions-).

    FeedResponse<Document> queryResults = this.client.queryDocuments(
        "/dbs/familydb/colls/familycoll",
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", 
        null);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }

## <a id="ReplaceDocument"></a>Passaggio 8: Sostituire un documento JSON
DocumentDB supporta l'aggiornamento di documenti JSON con il metodo [replaceDocument](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#replaceDocument-com.microsoft.azure.documentdb.Document-com.microsoft.azure.documentdb.RequestOptions-).

    // Update a property
    andersenFamily.Children[0].Grade = 6;

    this.client.replaceDocument(
        "/dbs/familydb/colls/familycoll/docs/Andersen.1", 
        andersenFamily,
        null);

## <a id="DeleteDocument"></a>Passaggio 9: Eliminare un documento JSON
Analogamente, DocumentDB supporta l'eliminazione di documenti JSON con il metodo [deleteDocument](http://azure.github.io/azure-documentdb-java/com/microsoft/azure/documentdb/DocumentClient.html#deleteDocument-java.lang.String-com.microsoft.azure.documentdb.RequestOptions-).  

    this.client.delete("/dbs/familydb/colls/familycoll/docs/Andersen.1", null);

## <a id="DeleteDatabase"></a>Passaggio 10: Eliminare il database
Se si elimina il database creato, insieme al database vengono rimosse tutte le risorse figlio, come raccolte, documenti e così via.

    this.client.deleteDatabase("/dbs/familydb", null);

## <a id="Run"></a>Passaggio 11: Eseguire l'intera applicazione console Java
Per eseguire l'applicazione dalla console, eseguire prima la compilazione con Maven:
    
    mvn package

L'esecuzione di `mvn package` scarica la libreria di DocumentDB più recente da Maven e produce `GetStarted-0.0.1-SNAPSHOT.jar`. Eseguire l'app usando quanto segue:

    mvn exec:java -D exec.mainClass=GetStarted.Program

Congratulazioni. L'esercitazione su NoSQL è stata completata ed è stata creata un'applicazione console Java funzionante.

## <a name="next-steps"></a>Passaggi successivi
* Per un'esercitazione su app Web Java, vedere [Creazione di un'applicazione Web Java con DocumentDB](documentdb-java-application.md).
* Informazioni su come [monitorare un account DocumentDB](documentdb-monitor-accounts.md).
* Eseguire query sul set di dati di esempio illustrato nella pagina [Query Playground](https://www.documentdb.com/sql/demo).
* Per altre informazioni sul modello di programmazione, vedere la sezione relativa allo sviluppo nella pagina [Documentazione di DocumentDB](https://azure.microsoft.com/documentation/services/documentdb/).

[documentdb-create-account]: documentdb-create-account.md
[keys]: media/documentdb-get-started/nosql-tutorial-keys.png

