---
title: Componenti e versioni di Hadoop - Azure HDInsight | Documentazione Microsoft
description: Informazioni sui componenti e sulle versioni di Hadoop in HDInsight e sui livelli di servizio disponibili in questa distribuzione cloud di HortonWorks Data Platform.
services: hdinsight
editor: cgronlun
manager: asadk
author: bprakash
tags: azure-portal
documentationcenter: 
ms.assetid: 367b3f4a-f7d3-4e59-abd0-5dc59576f1ff
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/14/2017
ms.author: bprakash
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: 990ac507c0d0f26483dc0db7ec4bce793100cb60
ms.lasthandoff: 04/15/2017


---
# <a name="what-are-the-different-hadoop-components-and-versions-available-with-hdinsight"></a>Componenti e versioni di Hadoop disponibili in HDInsight

Informazioni sui livelli di servizio offerti per Azure HDInsight e su componenti e versioni dell'ecosistema Hadoop. Ogni versione di HDInsight è una distribuzione cloud di una versione di HortonWorks Data Platform (HDP).

## <a name="hdinsight-standard-and-hdinsight-premium"></a>HDInsight Standard e HDInsight Premium

Azure HDInsight presenta le offerte cloud per i Big Data in due categorie: **Standard** e **Premium**. La tabella seguente elenca le funzionalità disponibili **solo come parte della categoria Premium**. Le funzionalità non indicate esplicitamente nella tabella di seguito sono disponibili come parte della categoria Standard.

> [!NOTE]
> Attualmente l’offerta HDInsight Premium è disponibile in anteprima e solo per i cluster Linux.
>
>

| Funzionalità HDInsight Premium | Description |
| --- | --- |
| Cluster HDInsight aggiunti al dominio |Aggiungere i cluster HDInsight ai domini di Azure Active Directory (AAD) per ottenere una sicurezza di livello aziendale. È ora possibile configurare un elenco di dipendenti dell'azienda autorizzati a eseguire l'autenticazione tramite Azure Active Directory per accedere al cluster HDInsight. L'amministratore può anche configurare il controllo degli accessi in base al ruolo per la sicurezza di Hive usando [Apache Ranger](http://hortonworks.com/apache/ranger/), limitando così l'accesso ai dati solo ai ruoli interessati. Infine, l'amministratore può controllare l'accesso ai dati da parte dei dipendenti e le eventuali modifiche apportate ai criteri di controllo degli accessi, ottenendo in questo modo un elevato livello di governance delle risorse aziendali. Per altre informazioni, vedere [Configure domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md) (Configurare i cluster HDInsight aggiunti al dominio). |

### <a name="cluster-types-supported-for-hdinsight-premium"></a>Tipi di cluster supportati per HDInsight Premium
Nella tabella seguente sono elencati il tipo di cluster HDInsight e la matrice di supporto Premium.

| Tipo di cluster | Standard | Premium (anteprima) |
| --- | --- | --- |
| Hadoop |Sì |Sì (solo HDInsight 3.5 e 3.6) |
| Spark |Sì |No |
| HBase |Sì |No |
| Storm |Sì |No |
| R Server  |Sì |No |
| Interactive Hive (anteprima) |Sì |No |
| Kafka (anteprima)|Sì|No| 

Questa tabella verrà aggiornata man mano che vengono inclusi altri cluster in HDInsight Premium.

### <a name="features-not-supported-for-hdinsight-premium"></a>Funzionalità non supportate per HDInsight Premium

Le seguenti funzionalità non sono attualmente supportate per i cluster HDInsight Premium.

* **Azure Data Lake Store non è supportato come risorsa di archiviazione principale**. È comunque possibile usare Azure Data Lake Store come risorsa di archiviazione aggiuntiva con i cluster HDInsight Premium.

### <a name="pricing-and-sla"></a>Prezzi e contratto di servizio
Per informazioni su prezzi e contratto di servizio per HDInsight Premium, vedere [Prezzi di HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="hadoop-components-available-with-different-hdinsight-versions"></a>Componenti di Hadoop disponibili con diverse versioni di HDInsight
Azure HDInsight supporta più versioni cluster di Hadoop che possono essere distribuite in qualsiasi momento. Ogni versione scelta crea una versione specifica della distribuzione HDP (Hortonworks Data Platform) e un set di componenti contenuti in tale distribuzione. Le versioni del componente associate alle versioni del cluster HDInsight sono elencate nella tabella seguente. Notare che la versione cluster predefinita usata da Azure HDInsight è attualmente la 3.5 e, a partire dal 17/02/2017, è basata su HDP 2.5.

> [!NOTE]
> La versione predefinita del servizio può cambiare senza preavviso. Se si dispone di una dipendenza della versione, è consigliabile specificare la versione quando si crea il cluster tramite l'SDK .NET, Azure PowerShell e l'interfaccia della riga di comando di Azure.
>
>


| Componente | HDInsight versione 3.6 | HDInsight versione 3.5 (predefinita) | HDInsight versione 3.4 | HDInsight versione 3.3 | HDInsight versione 3.2 | HDInsight versione 3.1 | HDInsight versione 3.0 |
| --- | --- | --- | --- | --- | --- | --- |--- |
| Hortonworks Data Platform |2.6 |2.5 |2.4 |2.3 |2.2 |2.1.7 |2.0 |
| Apache Hadoop e YARN |2.7.3 |2.7.3 |2.7.1 |2.7.1 |2.6.0 |2.4.0 |2.2.0 |
| Apache Tez |0.7.0 |0.7.0 |0.7.0 |0.7.0 |0.5.2 |0.4.0 |-|
| Apache Pig |0.16.0 |0.16.0 |0.15.0 |0.15.0 |0.14.0 |0.12.1 |0.12.0 |
| Apache Hive e HCatalog |1.2.1 |1.2.1 |1.2.1 |1.2.1 |0.14.0 |0.13.1 |0.12.0 |
| Apache Hive2 | 2.1.0 |-|-|-|-|-|-|
| Apache Tez-Hive2 | 0.8.4 |-|-|-|-|-|-|
| Apache Ranger | 0.7.0 |0.6.0 |-|-|-|-|-|
| Apache HBase |1.1.2 |1.1.2 |1.1.2 |1.1.1 |0.98.4 |0.98.0 |-|
| Apache Sqoop |1.4.6 |1.4.6 |1.4.6 |1.4.6 |1.4.5 |1.4.4 |1.4.4 |
| Apache Oozie |4.2.0 |4.2.0 |4.2.0 |4.2.0 |4.1.0 |4.0.0 |4.0.0 |
| Apache Zookeeper |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.6 |3.4.5 |3.4.5 |
| Apache Storm |1.1.0 |1.0.1 |0.10.0 |0.10.0 |0.9.3 |0.9.1 |-|
| Apache Mahout |0.9.0+ |0.9.0+ |0.9.0+ |0.9.0+ |0.9.0 |0.9.0 |-|
| Apache Phoenix |4.7.0 |4.7.0 |4.4.0 |4.4.0 |4.2.0 |4.0.0.2.1.7.0-2162 |-|
| Apache Spark |2.1.0 (solo Linux) |1.6.2 + 2.0 (solo Linux) |1.6.0 (solo Linux) |1.5.2 (solo Linux/build sperimentale) |1.3.1 (solo Windows) |-|-|
| Apache Kafka | 0.10.0 | 0.10.0 | 0.9.0 |-|-|-|-|
| Apache Ambari | 2.5.0 | 2.4.0 | 2.2.1 | 2.1.0 |-|-|-|
| Apache Zeppelin | 0.7.0 |-|-|-|-|-|-|
| Mono |4.2.1 |4.2.1 |3.2.8 |-|-|-|

**Ottenere le informazioni sulla versione del componente corrente**

Le versioni dei componenti associate alle versioni cluster HDInsight potrebbero subire modifiche nei futuri aggiornamenti a HDInsight. Per determinare i componenti disponibili e verificare le versioni in uso per un cluster, usare l'API REST Ambari. È possibile usare il comando **GetComponentInformation** per recuperare informazioni su un componente del servizio. Per informazioni dettagliate, vedere la [documentazione di Ambari][ambari-docs]. Per ottenere queste informazioni, è anche possibile accedere a un cluster usando Desktop remoto ed esaminare direttamente il contenuto della directory "C:\apps\dist\".

**Note sulla versione**

Per altre note sulla versione relative alle versioni più recenti di HDInsight, vedere [Note sulla versione di HDInsight](hdinsight-release-notes.md) .

## <a name="supported-hdinsight-versions"></a>Versioni supportate di HDInsight
La tabella seguente include l'elenco delle versioni di HDInsight attualmente disponibili, le corrispondenti versioni HDP (Hortonworks Data Platform) usate e le date di rilascio. Se note, vengono indicate anche la data di scadenza del supporto e la data in cui le versioni sono deprecate. Tenere presente quanto segue:

* i cluster ad alta disponibilità con due nodi head vengono distribuiti per impostazione predefinita per HDInsight 2.1 e versioni successive. Non sono disponibili per i cluster HDInsight 1.6.
* Dopo che il supporto per una particolare versione è scaduto, potrebbe non essere disponibile tramite il portale di Azure. Nella tabella seguente sono indicate le versioni disponibili sul portale di Azure classico. Le versioni cluster continueranno a essere disponibili usando il parametro `Version` nel comando [New-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619331.aspx) di Windows PowerShell e .NET SDK fino alla data di dichiarazione di obsolescenza.

| HDInsight Version | Versione HDP | Sistema operativo della macchina virtuale | Disponibilità elevata | Data di rilascio | Disponibile nel portale di Azure | Data di scadenza del supporto | Data di dichiarazione obsolescenza |
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDI 3.6 |HDP 2.6 |Ubuntu 16 |Sì |04/06/2017 |Sì | | |
| HDI 3.5 |HDP 2.5 |Ubuntu 16 |Sì |9/30/2016 |Sì |07/05/2017 |05/31/2018 |
| HDI 3.4 |HDP 2.4 |Ubuntu 14.0.4 LTS |Sì |29/03/2016 |Sì |12/29/2016 |1/9/2018 |
| HDI 3.3 |HDP 2.3 |Ubuntu 14.0.4 LTS o Windows Server 2012R2 |Sì |02/12/2015 |Sì |27/06/2016 |31/07/2017 |
| HDI 3.2 |HDP 2.2 |Ubuntu 12.04 LTS o Windows Server 2012R2 |Sì |2/18/2015 |No |01/03/2016 |01/04/2017 |
| HDI 3.1 |HDP 2.1 |Windows Server 2012 R2 |Sì |6/24/2014 |No |18/05/2015 |30/06/2016 |
| HDI 3.0 |HDP 2.0 |Windows Server 2012 R2 |Sì |11/02/2014 |No |17/09/2014 |30/06/2015 |
| HDI 2.1 |HDP 1.3 |Windows Server 2012 R2 |Sì |28/10/2013 |No |12/05/2014 |31/05/2015 |
| HDI 1.6 |HDP 1.1 | |No |28/10/2013 |No |26/04/2014 |31/05/2015 |

##<a name="hdi-version-33-nearing-deprecation-date"></a>HDI versione 3.3 prossima alla data in cui verrà dichiarata deprecata
Il supporto per il cluster HDI 3.3 è scaduto in data 27/06/2016 e verrà dichiarato deprecato in data 31/07/2017. Se si dispone di un cluster HDI 3.3, aggiornarlo a HDI 3.5 o HDI 3.6. Le sequenze temporali di deprecazione per Windows HDI 3.3 possono variare in base all'area geografica. I clienti riceveranno una comunicazione separata se la data in cui è prevista che si dichiari deprecata nella loro area è diversa da quella identificata nella presente comunicazione.

### <a name="the-service-level-agreement-for-hdinsight-cluster-versions"></a>Contratto di servizio per le versioni dei cluster HDInsight
Il Contratto di servizio viene definito come "finestra di supporto". Il termine finestra di supporto indica il periodo di tempo in cui una versione cluster HDInsight è supportata dal Supporto tecnico Microsoft. Un cluster HDInsight non è compreso nella finestra di supporto se la **data di scadenza del supporto** della versione ha superato la data corrente. Nella tabella precedente è disponibile un elenco di versioni di cluster HDInsight supportate. La data di scadenza del supporto per una determinata versione di HDInsight X (una volta che sarà disponibile una versione X+1 più recente) viene calcolata come l'ultima di:  

* Formula 1: aggiungere 180 giorni alla data di rilascio del cluster HDInsight versione X.
* Formula 2: aggiungere 90 giorni alla data in cui il cluster HDInsight versione X+1 (la versione successiva a X) diventa disponibile nel portale.

**Data in cui è deprecata** è la data dopo la quale non è possibile creare la versione del cluster su HDInsight. A partire dal 31 luglio 2017 non è possibile ridimensionare un cluster dopo la data di scadenza del supporto. 

> [!NOTE]
> Il cluster HDInsight basato su Windows (incluse le versioni 2.1, 3.0, 3.1, 3.2 e 3.3) esegue il sistema operativo guest di Azure Family 4, che usa la versione a 64 bit di Windows Server 2012 R2 e supporta .NET Framework 4.0, 4.5, 4.5.1 e 4.5.2.
>
>

## <a name="hortonworks-release-notes-associated-with-hdinsight-versions"></a>Note sulla versione di Hortonworks associate alle versioni di HDInsight
* Il cluster HDInsight versione 3.4 usa una distribuzione Hadoop basata su [Hortonworks Data Platform 2.4](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html). Si tratta del cluster Hadoop **predefinito** creato mediante il portale.
* Il cluster HDInsight versione 3.3 usa una distribuzione Hadoop basata su [Hortonworks Data Platform 2.3](http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html).

  * Le note sulla versione di Apache Storm sono disponibili [qui](https://storm.apache.org/2015/11/05/storm0100-released.html).
  * Le note sulla versione di Apache Hive sono disponibili [qui](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12332384&styleName=Text&projectId=12310843).
* Il cluster HDInsight versione 3.2 usa una distribuzione Hadoop basata su [Hortonworks Data Platform 2.2][hdp-2-2].  

  * Note sulla versione per componenti specifici di Apache: [Hive 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310843&version=12326450), [Pig 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310730&version=12326954), [HBase 0.98.4](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310753&version=12326810), [Phoenix 4.2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12315120&version=12327581), [M/R 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310941&version=12327180), [HDFS 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310942&version=12327181), [YARN 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12313722&version=12327197), [Common](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310240&version=12327179), [Tez 0.5.2](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314426&version=12328742), [Ambari 2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12312020&version=12327486), [Storm 0.9.3](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314820&version=12327112), [Oozie 4.1.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12324960&projectId=12311620).
* Il cluster HDInsight versione 3.1 usa una distribuzione Hadoop basata su [Hortonworks Data Platform 2.1.7][hdp-2-1-7]. I cluster HDInsight 3.1 creati prima del 7/11/2014 sono basati su [Hortonworks Data Platform 2.1.1][hdp-2-1-1].
* Il cluster HDInsight versione 3.0 usa una distribuzione Hadoop basata su [Hortonworks Data Platform 2.0][hdp-2-0-8].
* Il cluster HDInsight versione 2.1 usa una distribuzione Hadoop basata su [Hortonworks Data Platform 1.3][hdp-1-3-0].
* Il cluster HDInsight versione 1.6 usa una distribuzione Hadoop basata su [Hortonworks Data Platform 1.1][hdp-1-1-0].

[image-hdi-versioning-versionscreen]: ./media/hdinsight-component-versioning/hdi-versioning-version-screen.png

[wa-forums]: http://azure.microsoft.com/support/forums/

[connect-excel-with-hive-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md

[hdp-2-2]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.0/HDP_2.2.0_Release_Notes_20141202_version/index.html

[hdp-2-1-7]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: http://docs.hortonworks.com/HDPDocuments/HDP1/HDP-Win-1.1/bk_releasenotes_HDP-Win/content/ch_relnotes-hdp-win-1.1.0_1.html

[ambari-docs]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[zookeeper]: http://zookeeper.apache.org/

