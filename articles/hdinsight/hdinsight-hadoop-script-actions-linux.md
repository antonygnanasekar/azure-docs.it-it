---
title: Sviluppo di azioni script con HDInsight basato su Linux| Documentazione Microsoft
description: 'Informazioni su come personalizzare cluster HDInsight basati su Linux con Azioni script. Le azioni script consentono di personalizzare i cluster Azure HDInsight specificando le impostazioni di configurazione del cluster oppure installando servizi, strumenti o altri componenti software nel cluster. '
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: cf4c89cd-f7da-4a10-857f-838004965d3e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 9068e0e92e15491d3377a1b8f42071b56373396e
ms.lasthandoff: 04/12/2017


---
# <a name="script-action-development-with-hdinsight"></a>Sviluppo di azioni script con HDInsight

Le azioni script consentono di personalizzare i cluster Azure HDInsight specificando le impostazioni di configurazione del cluster oppure installando servizi, strumenti o altri componenti software nel cluster. È possibile usare azioni script durante la creazione del cluster oppure in un cluster in esecuzione.

> [!IMPORTANT]
> I passaggi descritti in questo documento richiedono un cluster HDInsight che usa Linux. Linux è l'unico sistema operativo usato in HDInsight versione 3.4 o successiva. Per altre informazioni, vedere [HDInsight deprecato in Windows](hdinsight-component-versioning.md#hdi-version-33-nearing-deprecation-date).

## <a name="what-are-script-actions"></a>Definizione di azioni script

Le azioni script sono script Bash eseguiti da Azure sui nodi del cluster per apportare modifiche alla configurazione o installare software. Un'azione script viene eseguita come radice e fornisce diritti di accesso completo ai nodi del cluster.

L'azione script può essere applicata usando i metodi seguenti:

| Usare per applicare uno script... | Durante la creazione di un cluster... | In un cluster in esecuzione... |
| --- |:---:|:---:|
| Portale di Azure |✓ |✓ |
| Azure PowerShell |✓ |✓ |
| Interfaccia della riga di comando di Azure |&nbsp; |✓ |
| HDInsight .NET SDK |✓ |✓ |
| Modello di Azure Resource Manager |✓ |&nbsp; |

Per altre informazioni sull'uso di questi metodi per l'applicazione di azioni script, vedere [Personalizzare cluster HDInsight tramite azioni script](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="bestPracticeScripting"></a>Procedure consigliate per lo sviluppo di script

Quando si sviluppa uno script personalizzato per un cluster HDInsight, è opportuno seguire le procedure consigliate indicate di seguito:

* [Usare la versione di Hadoop](#bPS1)
* [Usare la versione del sistema operativo](#bps10)
* [Fornire collegamenti stabili alle risorse di script](#bPS2)
* [Usare risorse precompilate](#bPS4)
* [Assicurarsi che lo script di personalizzazione del cluster sia idempotente](#bPS3)
* [Verificare la disponibilità elevata dell'architettura del cluster](#bPS5)
* [Configurare i componenti personalizzati per l'uso dell'archivio BLOB di Azure](#bPS6)
* [Scrivere informazioni in STDOUT e STDERR](#bPS7)
* [Salvare i file in formato ASCII con terminazioni di riga LF](#bps8)
* [Usare la logica di ripetizione dei tentativi per il ripristino da errori temporanei](#bps9)

> [!IMPORTANT]
> Le azioni di script devono essere completate entro 60 minuti; in caso contrario si verifica un timeout. Durante il provisioning dei nodi, lo script viene eseguito contemporaneamente ad altri processi di installazione e configurazione. In caso di concorrenza per risorse come il tempo di CPU o la larghezza di banda di rete, lo script può richiedere più tempo per completare l'operazione rispetto al tempo che impiegherebbe in un ambiente di sviluppo.

### <a name="bPS1"></a>Usare la versione di Hadoop

Nelle diverse versioni di HDInsight sono installate versioni diverse di servizi e componenti di Hadoop. Se lo script prevede una versione specifica di un servizio o un componente, si dovrà usare lo script solo con la versione di HDInsight che include i componenti richiesti. Per trovare informazioni sulle versioni dei componenti incluse in HDInsight, usare il documento relativo al [controllo delle versioni dei componenti di HDInsight](hdinsight-component-versioning.md) .

### <a name="bps10"></a> Usare la versione del sistema operativo

HDInsight basato su Linux si basa sulla distribuzione di Ubuntu Linux. Versioni diverse di HDInsight si basano su versioni differenti di Ubuntu e ciò può influire sul comportamento dello script. HDInsight 3.4 e versioni precedenti si basano ad esempio su versioni di Ubuntu che usano Upstart. La versione 3.5 si basa su Ubuntu 16.04 che usa Systemd. Systemd e Upstart si basano su comandi diversi, quindi lo script deve essere scritto in modo da funzionare con entrambi.

Un'altra differenza importante tra HDInsight 3.4 e 3.5 è che `JAVA_HOME` punta ora a Java 8.

È possibile verificare la versione del sistema operativo con `lsb_release`. I frammenti di codice seguenti dello script di installazione Hue illustrano come determinare se lo script è in esecuzione su Ubuntu 14 o 16:

```bash
OS_VERSION=$(lsb_release -sr)
if [[ $OS_VERSION == 14* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
    HUE_TARFILE=hue-binaries-14-04.tgz
elif [[ $OS_VERSION == 16* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
    HUE_TARFILE=hue-binaries-16-04.tgz
fi
...
if [[ $OS_VERSION == 16* ]]; then
    echo "Using systemd configuration"
    systemctl daemon-reload
    systemctl stop webwasb.service    
    systemctl start webwasb.service
else
    echo "Using upstart configuration"
    initctl reload-configuration
    stop webwasb
    start webwasb
fi
...
if [[ $OS_VERSION == 14* ]]; then
    export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64
elif [[ $OS_VERSION == 16* ]]; then
    export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
fi
```

Lo script completo contenente questi frammenti è disponibile all'indirizzo https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh.

Per la versione di Ubuntu usata da HDInsight, vedere il documento sulle [versioni dei componenti HDInsight](hdinsight-component-versioning.md).

Per comprendere le differenze tra Systemd e Upstart, vedere [Systemd per gli utenti di Upstart](https://wiki.ubuntu.com/SystemdForUpstartUsers).

### <a name="bPS2"></a>Fornire collegamenti stabili alle risorse di script

È necessario assicurarsi che gli script e tutte le risorse usati dallo script rimangano disponibili per l'intero ciclo di vita del cluster stesso e che durante tale periodo le versioni di questi file non cambino. Queste risorse sono necessarie se vengono aggiunti nuovi nodi al cluster durante operazioni di ridimensionamento.

È consigliabile scaricare e archiviare tutti gli elementi in un account di archiviazione di Azure nella propria sottoscrizione.

> [!IMPORTANT]
> L'account di archiviazione usato deve essere quello predefinito per il cluster o un contenitore pubblico di sola lettura per qualsiasi altro account di archiviazione.

Gli esempi forniti da Microsoft, ad esempio, vengono archiviati nell'account di archiviazione [https://hdiconfigactions.blob.core.windows.net/](https://hdiconfigactions.blob.core.windows.net/) , un contenitore pubblico di sola lettura gestito dal team di HDInsight.

### <a name="bPS4"></a>Usare risorse precompilate

Per ridurre il tempo necessario per eseguire lo script, evitare operazioni di compilazione delle risorse dal codice sorgente. Al contrario, precompilare le risorse e archiviare la versione binaria nell'archivio BLOB di Azure, in modo che possa essere rapidamente scaricato nel cluster dallo script.

### <a name="bPS3"></a>Assicurarsi che lo script di personalizzazione del cluster sia idempotente

Gli script devono essere progettati in modo che siano idempotenti. In altri termini, se lo script viene eseguito più volte, lo script deve assicurare che, a ogni esecuzione, il cluster venga riportato allo stesso stato.

Se, ad esempio, al momento della prima esecuzione uno script personalizzato installa un'applicazione in /usr/local/bin, a ogni esecuzione successiva dovrà verificare se l'applicazione esiste già nel percorso /usr/local/bin prima di procedere con altri passaggi.

### <a name="bPS5"></a>Verificare la disponibilità elevata dell'architettura del cluster

I cluster HDInsight basati su Linux forniscono due nodi head attivi all'interno del cluster e le azioni script vengono eseguite per entrambi i nodi. Se i componenti installati prevedono un solo nodo head, è necessario progettare uno script che installi il componente solo in uno dei due nodi del cluster.

> [!IMPORTANT]
> I servizi predefiniti installati come parte di HDInsight sono progettati per il failover tra i due nodi head, se necessario. Questa funzionalità non è tuttavia estesa ai componenti personalizzati installati tramite le azioni script. Se i componenti installati tramite un'azione script richiedono la disponibilità elevata, è necessario implementare un meccanismo di failover personalizzato per l'uso dei due nodi head disponibili.

### <a name="bPS6"></a>Configurare i componenti personalizzati per l'uso dell'archivio BLOB di Azure

I componenti installati nel cluster possono avere una configurazione predefinita che usa l'archiviazione di Hadoop Distributed File System (HDFS). HDInsight usa l'archiviazione BLOB di Azure come risorsa di archiviazione predefinita, in modo da offre un file system compatibile con HDFS che rende permanenti i dati anche se il cluster viene eliminato. È consigliabile configurare i componenti installati in modo che usino WASB invece di HDFS.

Ad esempio, il codice seguente copia il file giraph-examples.jar dal file system locale in WASB:

```bash
hdfs dfs -put /usr/hdp/current/giraph/giraph-examples.jar /example/jars/
```

### <a name="bPS7"></a>Scrivere informazioni in STDOUT e STDERR

Le informazioni scritte in STDOUT e STDERR durante l'esecuzione dello script vengono registrate e possono essere visualizzate tramite l'interfaccia utente Web di Ambari.

> [!NOTE]
> Ambari sarà disponibile solo se il cluster viene creato correttamente. Se si usa un'azione script durante la creazione del cluster e la creazione ha esito negativo, vedere la sezione relativa alla risoluzione dei problemi nell'articolo [Personalizzare cluster HDInsight basati su Linux tramite Azione script](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting) che illustra altri modi per accedere alle informazioni registrate.

La maggior parte delle utilità e dei pacchetti di installazione scrivono già le informazioni in STDOUT e STDERR, tuttavia è possibile aggiungere altre opzioni di registrazione. Per inviare testo a STDOUT, usare `echo`. ad esempio:

```bash
echo "Getting ready to install Foo"
```

Per impostazione predefinita, `echo` invia la stringa a STDOUT. Per indirizzarla a STDERR, aggiungere `>&2` prima di `echo`. ad esempio:

```bash
>&2 echo "An error occurred installing Foo"
```

Questo codice reindirizza le informazioni inviate a STDOUT (1 è il valore predefinito, quindi non indicato qui) a STDERR (2). Per altre informazioni sul reindirizzamento I/O, vedere [http://www.tldp.org/LDP/abs/html/io-redirection.html](http://www.tldp.org/LDP/abs/html/io-redirection.html).

Per altre informazioni sulla visualizzazione delle informazioni registrate tramite azioni script, vedere [Personalizzare cluster HDInsight basati su Linux tramite Azione script](hdinsight-hadoop-customize-cluster-linux.md#troubleshooting)

### <a name="bps8"></a> Salvare i file in formato ASCII con terminazioni di riga LF

Gli script Bash devono essere archiviati nel formato ASCII con righe terminate da LF. Se i file vengono archiviati nel formato UTF-8, che può includere un byte order mark all'inizio del file, oppure con terminazioni di riga CRLF, comuni negli editor di Windows, lo script non riuscirà con errori simili al seguente:

```
$'\r': command not found
line 1: #!/usr/bin/env: No such file or directory
```

### <a name="bps9"></a> Usare la logica di ripetizione dei tentativi per il ripristino da errori temporanei

Quando si scaricano file, l'installazione di pacchetti tramite apt-get o altre azioni che trasmettono dati su Internet, l'azione potrebbe non riuscire a causa di errori di rete temporanei. Ad esempio, è possibile che sia in corso il failover a un nodo di backup della risorsa remota con la quale si sta comunicando.

Per rendere lo script resiliente agli errori temporanei, è possibile implementare la logica di ripetizione dei tentativi. Di seguito è riportato un esempio una funzione che esegue qualsiasi comando ricevuto e, se il comando non riesce, ripete i tentativi per un massimo di tre volte. Attenderà due secondi tra ogni tentativo.

```bash
#retry
MAXATTEMPTS=3

retry() {
    local -r CMD="$@"
    local -i ATTMEPTNUM=1
    local -i RETRYINTERVAL=2

    until $CMD
    do
        if (( ATTMEPTNUM == MAXATTEMPTS ))
        then
                echo "Attempt $ATTMEPTNUM failed. no more attempts left."
                return 1
        else
                echo "Attempt $ATTMEPTNUM failed! Retrying in $RETRYINTERVAL seconds..."
                sleep $(( RETRYINTERVAL ))
                ATTMEPTNUM=$ATTMEPTNUM+1
        fi
    done
}
```

Ecco alcuni esempi d'uso di questa funzione.

```bash
retry ls -ltr foo

retry wget -O ./tmpfile.sh https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
```

## <a name="helpermethods"></a>Metodi helper per gli script personalizzati

I metodi helper di Azione script sono utilità che è possibile usare durante la scrittura di script personalizzati. Questi metodi sono definiti nella pagina all'indirizzo [https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh)e possono essere inclusi negli script mediante la seguente procedura:

```bash
# Import the helper method module.
wget -O /tmp/HDInsightUtilities-v01.sh -q https://hdiconfigactions.blob.core.windows.net/linuxconfigactionmodulev01/HDInsightUtilities-v01.sh && source /tmp/HDInsightUtilities-v01.sh && rm -f /tmp/HDInsightUtilities-v01.sh
```

Questo codice rende disponibili gli helper seguenti per l'uso nello script personalizzato:

| Utilizzo dell'helper | Descrizione |
| --- | --- |
| `download_file SOURCEURL DESTFILEPATH [OVERWRITE]` |Scarica un file dall'URI di origine al percorso file specificato. Per impostazione predefinita, non sovrascriverà un file esistente. |
| `untar_file TARFILE DESTDIR` |Estrae un file TAR (usando `-xf`) nella directory di destinazione. |
| `test_is_headnode` |Se viene eseguito su un nodo head del cluster restituisce 1; in caso contrario, 0. |
| `test_is_datanode` |Se il nodo corrente è un nodo dati (di lavoro) restituisce 1; in caso contrario, 0. |
| `test_is_first_datanode` |Se il nodo corrente è il primo nodo dati (di lavoro) (denominato workernode0) restituisce 1; in caso contrario, 0. |
| `get_headnodes` |Restituisce il nome di dominio completo dei nodi head nel cluster. I nomi sono delimitati da virgole. In caso di errore, viene restituita una stringa vuota. |
| `get_primary_headnode` |Ottiene il nome di dominio completo del nodo head primario. In caso di errore, viene restituita una stringa vuota. |
| `get_secondary_headnode` |Ottiene il nome di dominio completo del nodo head secondario. In caso di errore, viene restituita una stringa vuota. |
| `get_primary_headnode_number` |Ottiene il suffisso numerico del nodo head primario. In caso di errore, viene restituita una stringa vuota. |
| `get_secondary_headnode_number` |Ottiene il suffisso numerico del nodo head secondario. In caso di errore, viene restituita una stringa vuota. |

## <a name="commonusage"></a>Modelli di utilizzo comuni

Questa sezione fornisce indicazioni sull'implementazione di alcuni dei modelli di utilizzo comuni che si potrebbero riscontrare durante la scrittura dello script personalizzato.

### <a name="passing-parameters-to-a-script"></a>Passaggio di parametri a uno script

In alcuni casi, lo script potrebbe richiedere l'uso di parametri. Ad esempio, per recuperare informazioni dall'API REST di Ambari, potrebbe essere necessaria la password amministratore per il cluster.

I parametri passati allo script sono noti come *parametri posizionali* e vengono assegnati a `$1` per il primo parametro, a `$2` per il secondo e così via. `$0` contiene il nome dello script stesso.

I valori passati allo script come parametri devono essere racchiusi tra virgolette singole ('), in modo che il valore passato sia considerato un valore letterale e il carattere tra virgolette, ad esempio '!', non riceva un trattamento speciale.

### <a name="setting-environment-variables"></a>Impostazioni delle variabili di ambiente

L'impostazione di una variabile di ambiente viene eseguita nel modo seguente:

    VARIABLENAME=value

Dove VARIABLENAME è il nome della variabile. Per accedere poi alla variabile, usare `$VARIABLENAME`. Ad esempio, per assegnare un valore fornito da un parametro posizionale come variabile di ambiente denominata PASSWORD, usare la stringa seguente:

    PASSWORD=$1

Per il successivo accesso alle informazioni, si potrà quindi usare `$PASSWORD`.

Le variabili di ambiente impostate all'interno dello script sono disponibili solo nell'ambito dello script. In alcuni casi potrebbe essere necessario aggiungere variabili di ambiente a livello di sistema, che rimarranno persistenti dopo il completamento dello script. Di solito questo permette agli utenti che si connettono al cluster tramite SSH di usare i componenti installati dallo script. È possibile eseguire questa operazione aggiungendo la variabile di ambiente a `/etc/environment`. Il codice seguente aggiunge ad esempio **HADOOP\_CONF\_DIR**:

```bash
echo "HADOOP_CONF_DIR=/etc/hadoop/conf" | sudo tee -a /etc/environment
```

### <a name="access-to-locations-where-the-custom-scripts-are-stored"></a>Accedere alle posizioni in cui sono archiviati gli script personalizzati

Gli script usati per la personalizzazione di un cluster devono essere archiviati in una delle posizioni seguenti:

* __Account di archiviazione predefinito__ per il cluster.

* __Account di archiviazione aggiuntivo__ associato al cluster.

* __URI leggibile pubblicamente__, ad esempio OneDrive, Dropbox e così via.

* __Account Azure Data Lake Store__ associato al cluster HDInsight. Per altre informazioni sull'uso di Azure Data Lake Store con HDInsight, vedere [Creare un cluster HDInsight con Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

    > [!NOTE]
    > L'entità servizio usata da HDInsight per accedere a Data Lake Store deve avere accesso in lettura allo script.

Se lo script accede a risorse che si trovano altrove, queste devono essere in una posizione accessibile pubblicamente (almeno pubblica e di sola lettura). Ad esempio, è possibile scaricare un file nel cluster usando `download_file`.

L'archiviazione di un file in un account di archiviazione di Azure accessibile al cluster (ad esempio l'account di archiviazione predefinito) consentirà un accesso rapido, perché questo account di archiviazione si trova nella rete di Azure.

> [!NOTE]
> Il formato URI usato per fare riferimento allo script varia a seconda del servizio in uso. Per gli account di archiviazione associati al cluster HDInsight, usare `wasb://` o `wasbs://`. Per gli URI leggibili pubblicamente, usare `http://` o `https://`. Per Data Lake Store, usare `adl://`.

### <a name="checking-the-operating-system-version"></a>Verifica della versione del sistema operativo

Le diverse versioni di HDInsight si basano su versioni specifiche di Ubuntu. Ci possono essere differenze tra le versioni del sistema operativo che è necessario verificare nello script. Può essere ad esempio necessario installare un file binario associato alla versione di Ubuntu.

Per verificare la versione del sistema operativo usare `lsb_release`. Il codice seguente illustra ad esempio come fare riferimento a un file tar diverso a seconda della versione del sistema operativo:

```bash
OS_VERSION=$(lsb_release -sr)
if [[ $OS_VERSION == 14* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-14-04."
    HUE_TARFILE=hue-binaries-14-04.tgz
elif [[ $OS_VERSION == 16* ]]; then
    echo "OS verion is $OS_VERSION. Using hue-binaries-16-04."
    HUE_TARFILE=hue-binaries-16-04.tgz
fi
```

## <a name="deployScript"></a>Elenco di controllo per la distribuzione di un'azione script

Di seguito sono indicati i passaggi effettuati durante la preparazione della distribuzione degli script:

* Inserire i file che contengono gli script personalizzati in un percorso accessibile per i nodi del cluster durante la distribuzione. L'account di archiviazione può essere uno qualsiasi degli account predefiniti o aggiuntivi specificati durante la distribuzione del cluster oppure qualsiasi altro contenitore di archiviazione accessibile pubblicamente.
* Aggiungere controlli negli script per garantire che questi vengano eseguiti in modo idempotente per poter eseguire più volte lo script sullo stesso nodo.
* Usare una directory di file temporanei /tmp per conservare i file scaricati usati dagli script e quindi eliminarli dopo aver eseguito gli script.
* In caso di modifica delle impostazioni o dei file di configurazione del servizio Hadoop a livello di sistema operativo, è possibile riavviare i servizi HDInsight in modo che possano rilevare qualsiasi impostazione a livello di sistema operativo, ad esempio le variabili di ambiente impostate negli script.

## <a name="runScriptAction"></a>Come eseguire un'azione script

È possibile usare azioni script per personalizzare i cluster HDInsight con il portale di Azure, Azure PowerShell, i modelli di Azure Resource Manager o HDInsight .NET SDK. Per istruzioni, vedere [Come usare un'azione script](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="sampleScripts"></a>Esempi di script personalizzati

Microsoft fornisce script di esempio per installare i componenti in un cluster HDInsight. Gli script di esempio e le istruzioni su come usarli sono disponibili nei collegamenti seguenti:

* [Installare e usare Hue nei cluster HDInsight.](hdinsight-hadoop-hue-linux.md)
* [Installare e usare R nei cluster Hadoop HDInsight](hdinsight-hadoop-r-scripts-linux.md)
* [Installare e usare Solr nei cluster HDInsight](hdinsight-hadoop-solr-install-linux.md)
* [Installare e usare Giraph nei cluster HDInsight](hdinsight-hadoop-giraph-install-linux.md)  

> [!NOTE]
> I documenti indicati nei collegamenti precedenti sono specifici per i cluster HDInsight basati su Linux. Per gli script che funzionano con HDInsight basato su Windows, vedere [Sviluppo di azioni di script con HDInsight (Windows)](hdinsight-hadoop-script-actions.md) oppure usare i collegamenti disponibili all'inizio di ogni articolo.

## <a name="troubleshooting"></a>Risoluzione dei problemi

Di seguito sono elencati gli errori che potrebbero essere visualizzati quando si usano script personalizzati:

**Errore**: `$'\r': command not found`. A volte seguito da `syntax error: unexpected end of file`.

*Causa*: questo errore si verifica quando le righe di uno script terminano con CRLF. I sistemi Unix prevedono unicamente LF come terminazione di riga.

Questo problema si verifica più spesso quando lo script viene creato in un ambiente Windows, perché CRLF è una terminazione di riga comune per molti editor di testo in Windows.

*Risoluzione*: se nell'editor di testo è disponibile come opzione, selezionare il formato Unix o LF come terminazione di riga. È anche possibile usare i comandi seguenti in un sistema Unix per cambiare CRLF in LF:

> [!NOTE]
> I comandi seguenti sono all'incirca equivalenti nel senso che cambiano le terminazioni di riga CRLF in LF. Selezionarne uno in base alle utilità disponibili nel proprio sistema.

| Comando | Note |
| --- | --- |
| `unix2dos -b INFILE` |Verrà creata una copia di backup del file originale con estensione BAK |
| `tr -d '\r' < INFILE > OUTFILE` |OUTFILE conterrà una versione solo con le terminazioni LF |
| `perl -pi -e 's/\r\n/\n/g' INFILE` |Il file verrà modificato direttamente senza crearne uno nuovo |
| ```sed 's/$'"/`echo \\\r`/" INFILE > OUTFILE``` |OUTFILE conterrà una versione solo con le terminazioni LF. |

**Errore**: `line 1: #!/usr/bin/env: No such file or directory`.

*Causa*: questo errore si verifica quando lo script è stato salvato in formato UTF-8 con un byte order mark (BOM).

*Risoluzione*: salvare il file in formato ASCII o UTF-8 senza un carattere BOM. È anche possibile usare il comando seguente in un sistema Linux o Unix per creare un nuovo file senza il carattere BOM:

    awk 'NR==1{sub(/^\xef\xbb\xbf/,"")}{print}' INFILE > OUTFILE

Per il comando precedente sostituire **INFILE** con il file contenente il carattere BOM. **OUTFILE** deve essere un nuovo nome di file, che conterrà lo script senza il carattere BOM.

## <a name="seeAlso"></a>Passaggi successivi

* Informazioni su come [Personalizzare cluster HDInsight basati su Linux tramite Azione script](hdinsight-hadoop-customize-cluster-linux.md)
* Per informazioni sulla creazione di applicazioni .NET che gestiscono HDInsight, vedere [Riferimento .NET per HDInsight](https://msdn.microsoft.com/library/mt271028.aspx)
* Per informazioni su come usare REST per eseguire azioni di gestione nei cluster HDInsight, vedere l' [API REST del provider di risorse HDInsight](https://msdn.microsoft.com/library/azure/mt622197.aspx) .

