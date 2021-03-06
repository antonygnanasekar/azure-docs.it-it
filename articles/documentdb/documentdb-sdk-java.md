---
title: API Java, risorse e SDK per Azure DocumentDB | Microsoft Docs
description: Altre informazioni sulle date di rilascio e le date di ritiro dell&quot;SDK e dell&quot;API per Java e sulle modifiche apportate tra le versioni di DocumentDB Java SDK.
services: documentdb
documentationcenter: java
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 7861cadf-2a05-471a-9925-0fec0599351b
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 03/16/2017
ms.author: khdang
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 8c4e33a63f39d22c336efd9d77def098bd4fa0df
ms.openlocfilehash: 40ea65f692d1e2cbc39a6c65b2f8b255282e34cc
ms.lasthandoff: 04/20/2017


---
# <a name="documentdb-java-sdk-release-notes-and-resources"></a>DocumentDB Java SDK: note sulla versione e risorse
> [!div class="op_single_selector"]
> * [.NET](documentdb-sdk-dotnet.md)
> * [.NET Core](documentdb-sdk-dotnet-core.md)
> * [Node.js](documentdb-sdk-node.md)
> * [Java](documentdb-sdk-java.md)
> * [Python](documentdb-sdk-python.md)
> * [REST](https://docs.microsoft.com/en-us/rest/api/documentdb/)
> * [Provider di risorse REST](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td>**Download dell'SDK**</td><td>[Maven](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-documentdb%22)</td></tr>

<tr><td>**Documentazione sull'API**</td><td>[Documentazione di riferimento API Java](http://azure.github.io/azure-documentdb-java/)</td></tr>

<tr><td>**Contribuire all'SDK**</td><td>[GitHub](https://github.com/Azure/azure-documentdb-java/)</td></tr>

<tr><td>**Introduzione**</td><td>[Introduzione a SDK Java](documentdb-java-get-started.md)</td></tr>

<tr><td>**Esercitazione sull'app Web**</td><td>[Sviluppo di applicazioni Web con DocumentDB](documentdb-java-application.md)</td></tr>

<tr><td>**Runtime attualmente supportato**</td><td>[JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)</td></tr>
</table></br>

## <a name="release-notes"></a>Note sulla versione

### <a name="a-name11001100"></a><a name="1.10.0"/>1.10.0
* Supporto abilitato per la raccolta partizionata con valore ridotto di 2.500 UR/s e riduzione con incrementi di 100 UR/s.
* Correzione di un bug nell'assembly nativo che può causare l'eccezione NullRef in alcune query.

### <a name="a-name196196"></a><a name="1.9.6"/>1.9.6
* Correzione di un bug nella configurazione del motore di query che può generare eccezioni per le query in modalità Gateway.
* Correzione di alcuni bug nel contenitore della sessione che possono generare un'eccezione "Risorsa proprietario non trovata" per le richieste immediatamente dopo la creazione della raccolta.

### <a name="a-name195195"></a><a name="1.9.5"/>1.9.5
* Aggiunta del supporto per le query di aggregazione (COUNT, MIN, MAX, SUM e AVG). Vedere [Supporto dell'aggregazione](documentdb-sql-query.md#Aggregates).
* Aggiunta del supporto per la modifica del feed.
* Aggiunta del supporto per informazioni sulla quota della raccolta tramite RequestOptions.setPopulateQuotaInfo.
* Aggiunta del supporto per la registrazione dello script della procedura archiviata tramite RequestOptions.setScriptLoggingEnabled.
* Risoluzione di un bug in cui una query in modalità DirectHttps rischiava di bloccarsi in presenza di errori di limitazione.
* Risoluzione di un bug in modalità di coerenza di sessione.
* Risoluzione di un bug che potrebbe causare l'eccezione NullReferenceException in HttpContext quando la frequenza delle richieste è elevata.
* Miglioramento delle prestazioni della modalità DirectHttps.

### <a name="a-name194194"></a><a name="1.9.4"/>1.9.4
* Aggiunta del supporto del proxy basato su istanza di client semplice con l'API ConnectionPolicy.setProxy().
* Aggiunta dell'API DocumentClient.close() per arrestare correttamente l'istanza di DocumentClient.
* Miglioramento delle prestazioni delle query nella modalità di connettività diretta, tramite derivazione del piano di query dall'assembly nativo invece che dal gateway.
* Impostare FAIL_ON_UNKNOWN_PROPERTIES = false in modo che gli utenti non debbano definire JsonIgnoreProperties in POJO.
* Refactoring della registrazione per usare SLF4J.
* Risoluzione di altri bug nel lettore di coerenza.

### <a name="a-name193193"></a><a name="1.9.3"/>1.9.3
* Risoluzione di un bug nella gestione della connessione per impedire perdite di connessione nella modalità di connettività diretta.
* Risoluzione di un bug nella query TOP in cui è possibile che venga generata un'eccezione NullReferenece.
* Miglioramento delle prestazioni tramite la riduzione del numero di chiamate di rete per le cache interne.
* Aggiunta del codice di stato, di ActivityID e dell'URI della richiesta in DocumentClientException per una migliore risoluzione dei problemi.

### <a name="a-name192192"></a><a name="1.9.2"/>1.9.2
* Risoluzione di un problema nella gestione della connessione per una migliore stabilità.

### <a name="a-name191191"></a><a name="1.9.1"/>1.9.1
* Aggiunta del supporto per il livello di coerenza BoundedStaleness.
* Aggiunta del supporto per la connettività diretta per le operazioni CRUD delle raccolte partizionate.
* Risoluzione di un bug in una query di un database con SQL.
* Risoluzione di un bug nella cache della sessione in cui i token di sessione possono essere impostati in modo non corretto.

### <a name="a-name190190"></a><a name="1.9.0"/>1.9.0
* Aggiunta del supporto per le query in parallelo nelle raccolte partizionate.
* Aggiunta del supporto per le query TOP/ORDER BY nelle raccolte partizionate.
* Aggiunta del supporto per la coerenza assoluta.
* Aggiunta del supporto per le richieste basate su nomi quando si utilizza la connettività diretta.
* Correzione rendere ActivityId coerente tra tutti i tentativi di richiesta.
* Correzione del bug correlato alla cache di sessione quando si ricrea una raccolta con lo stesso nome.
* Aggiunta Polygon e LineString DataTypes quando si specifica il criterio di indicizzazione per la raccolta criteri per le query spaziali di geo-fencing.
* Risoluzione dei problemi con Java Doc per Java 1.8.

### <a name="a-name181181"></a><a name="1.8.1"/>1.8.1
* Risolto un bug in PartitionKeyDefinitionMap per memorizzare nella cache le raccolte a partizione singola ed evitare richieste aggiuntive di chiavi di partizione da recuperare.
* Risolto un bug per non eseguire un nuovo tentativo se viene fornito un valore di chiave di partizione errato.

### <a name="a-name180180"></a><a name="1.8.0"/>1.8.0
* Aggiunta del supporto per gli account di database con più aree.
* Aggiunta del supporto per la ripetizione automatica delle richieste limitate con la possibilità di personalizzare il numero massimo di tentativi e il relativo tempo di attesa massimo.  Vedere RetryOptions e ConnectionPolicy.getRetryOptions().
* IPartitionResolver basato su codice di partizionamento personalizzato è stato deprecato. Usare le raccolte partizionate per un'archiviazione e una velocità effettiva superiori.

### <a name="a-name171171"></a><a name="1.7.1"/>1.7.1
* Aggiunta del supporto per il criterio di ripetizione per la limitazione.  

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
* Aggiunta del supporto per la durata (TTL) relativa ai documenti.

### <a name="a-name160160"></a><a name="1.6.0"/>1.6.0
* Implementazione delle [raccolte partizionate](documentdb-partition-data.md) e dei [livelli di prestazioni definiti dall'utente](documentdb-performance-levels.md).

### <a name="a-name151151"></a><a name="1.5.1"/>1.5.1
* Risolto un bug in HashPartitionResolver per generare valori hash in little endian per coerenza con altri SDK.

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0
* Aggiungi resolver per partizioni hash e a intervalli come supporto per applicazioni di partizionamento orizzontale in più partizioni.

### <a name="a-name140140"></a><a name="1.4.0"/>1.4.0
* Implementazione di Upsert. Nuovi metodi upsertXXX aggiunti per supportare la funzionalità Upsert.
* Implementazione del routing basato su ID. Nessuna modifica API pubblica, tutte modifiche interne.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0
* Versione ignorata per riportare il numero di versione in allineamento con altri SDK

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Supporta l'indice geospaziale
* Convalida la proprietà id per tutte le risorse. Gli ID per le risorse non possono contenere i caratteri ?, /, #, \, o terminare con uno spazio.
* Aggiunge la nuova intestazione "stato di trasformazione dell'indice" a ResourceResponse.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* Implementa criteri di indicizzazione V2

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* SDK con disponibilità generale

## <a name="release--retirement-dates"></a>Date di rilascio e di ritiro
Microsoft invierà una notifica almeno **12 mesi** prima del ritiro di un SDK per agevolare la transizione a una versione più recente o supportata.

Le nuove caratteristiche e funzionalità e le ottimizzazioni vengono aggiunte solo all'SDK corrente, è quindi consigliabile eseguire sempre l'aggiornamento alla versione più recente dell'SDK quanto prima.

Qualsiasi richiesta inviata a DocumentDB con un SDK ritirato verrà rifiutata dal servizio.

> [!WARNING]
> Tutte le versioni dell'SDK per Java di Azure DocumentDB precedenti alla versione **1.0.0** verranno ritirate il **29 febbraio 2016**.
> 
> 

<br/>

| Versione | Data di rilascio | Data di ritiro |
| --- | --- | --- |
| [1.10.0](#1.10.0) |11 marzo 2017 |--- |
| [1.9.6](#1.9.6) |21 febbraio 2017 |--- |
| [1.9.5](#1.9.5) |31 gennaio 2017 |--- |
| [1.9.4](#1.9.4) |24 novembre 2016 |--- |
| [1.9.3](#1.9.3) |30 ottobre 2016 |--- |
| [1.9.2](#1.9.2) |28 ottobre 2016 |--- |
| [1.9.1](#1.9.1) |26 ottobre 2016 |--- |
| [1.9.0](#1.9.0) |03 ottobre 2016 |--- |
| [1.8.1](#1.8.1) |30 giugno 2016 |--- |
| [1.8.0](#1.8.0) |14 giugno 2016 |--- |
| [1.7.1](#1.7.1) |30 aprile 2016 |--- |
| [1.7.0](#1.7.0) |27 aprile 2016 |--- |
| [1.6.0](#1.6.0) |29 marzo 2016 |--- |
| [1.5.1](#1.5.1) |31 dicembre 2015 |--- |
| [1.5.0](#1.5.0) |04 dicembre 2015 |--- |
| [1.4.0](#1.4.0) |05 ottobre 2015 |--- |
| [1.3.0](#1.3.0) |05 ottobre 2015 |--- |
| [1.2.0](#1.2.0) |05 agosto 2015 |--- |
| [1.1.0](#1.1.0) |09 luglio 2015 |--- |
| [1.0.1](#1.0.1) |12 maggio 2015 |--- |
| [1.0.0](#1.0.0) |07 aprile 2015 |--- |
| 0.9.5-prelease |09 marzo 2015 |29 febbraio 2016 |
| 0.9.4-prelease |17 febbraio 2015 |29 febbraio 2016 |
| 0.9.3-prelease |13 gennaio 2015 |29 febbraio 2016 |
| 0.9.2-prelease |19 dicembre 2014 |29 febbraio 2016 |
| 0.9.1-prelease |19 dicembre 2014 |29 febbraio 2016 |
| 0.9.0-prelease |10 dicembre 2014 |29 febbraio 2016 |

## <a name="faq"></a>Domande frequenti
[!INCLUDE [documentdb-sdk-faq](../../includes/documentdb-sdk-faq.md)]

## <a name="see-also"></a>Vedere anche
Per altre informazioni su DocumentDB, vedere la pagina del servizio [Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) .


