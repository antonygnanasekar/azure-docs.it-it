---
title: Replica geografica attiva per il database SQL di Azure
description: La replica geografica attiva consente di configurare 4 repliche del database in qualsiasi data center di Azure.
services: sql-database
documentationcenter: na
author: anosov1960
manager: jhubbard
editor: monicar
ms.assetid: 2a29f657-82fb-4283-9a83-e14a144bfd93
ms.service: sql-database
ms.custom: business continuity
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: NA
ms.date: 09/26/2016
ms.author: sashan
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: 8d8743529f4f0b9e8c036d328d63bea247279fe3
ms.lasthandoff: 04/15/2017


---
# <a name="overview-sql-database-active-geo-replication"></a>Panoramica: Replica geografica attiva per il database SQL di Azure
La replica geografica attiva consente di configurare fino a quattro database secondari accessibili in lettura nella stessa posizione del data center o in posizioni (aree) diverse. Sono disponibili database secondari per l'esecuzione di query e il failover in caso di interruzione di un data center o dell'impossibilità di connettersi al database primario. La replica geografica attiva deve essere avvenire tra database della stessa sottoscrizione.

> [!NOTE]
> La replica geografica attiva (database secondari accessibili in lettura) è ora disponibile per tutti i database in tutti i livelli di servizio. Nell'aprile 2017 il tipo di database secondario non leggibile verrà ritirato e i database non leggibili esistenti verranno aggiornati automaticamente a database secondari accessibili in lettura.
>  

 È possibile configurare la replica geografica attiva usando il [portale di Azure](sql-database-geo-replication-portal.md), [PowerShell](scripts/sql-database-setup-geodr-and-failover-database-powershell.md), [Transact-SQL](sql-database-geo-replication-transact-sql.md) o [API REST - Creare o aggiornare il database](https://msdn.microsoft.com/library/azure/mt163685.aspx).

Se per qualsiasi motivo il database primario restituisce un errore o deve essere portato offline, è possibile *eseguire il failover* in uno dei database secondari. Una volta attivato il failover su uno dei database secondari, tutti gli altri database secondari vengono automaticamente collegati al nuovo database primario.

È possibile eseguire il failover su un database secondario tramite il [portale di Azure](sql-database-geo-replication-portal.md), [PowerShell](scripts/sql-database-setup-geodr-and-failover-database-powershell.md), [Transact-SQL](sql-database-geo-replication-failover-transact-sql.md), [API REST, Failover pianificato](https://msdn.microsoft.com/library/mt575007.aspx) o [API REST, Failover non pianificato](https://msdn.microsoft.com/library/mt582027.aspx).

Dopo il failover, verificare che nel nuovo database primario siano configurati i requisiti di autenticazione relativi al server e al database. Per tutti i dettagli, vedere l'articolo sulla [sicurezza del database SQL di Azure dopo il ripristino di emergenza](sql-database-geo-replication-security-config.md).

La funzionalità Replica geografica attiva implementa un meccanismo per garantire la ridondanza dei database nella stessa area di Microsoft Azure o in aree diverse (ridondanza geografica). La replica geografica attiva replica in modo asincrono le transazioni di cui è stato eseguito il commit da un database a un massimo di quattro copie del database in server diversi, usando l'isolamento RCSI (Read Committed Snapshot Isolation). Quando è configurata la replica geografica attiva, un database secondario viene creato nel server specificato. Il database originale diventa il database primario. Il database primario replica in modo asincrono le transazioni sottoposte a commit in ognuno dei database secondari. Vengono replicate solo le transazioni complete. Anche se a un certo punto i dati del database secondario possono essere leggermente indietro rispetto al database primario, per tali dati sono sempre garantite transazioni complete. È possibile consultare i dati RPO specifici in [Panoramica della continuità aziendale](sql-database-business-continuity.md).

Uno dei principali vantaggi offerti dalla replica geografica attiva è quello di fornire una soluzione di ripristino di emergenza a livello di database con tempi di ripristino ridotti. Quando si inserisce il database secondario in un server in un'area diversa, viene aggiunta la massima flessibilità all'applicazione. La ridondanza tra aree consente il ripristino delle applicazioni dalla perdita definitiva di un intero data center o di parti di esso causata da calamità naturali, errori umani irreversibili o atti dolosi. La figura seguente illustra un esempio di replica geografica attiva configurata con il database primario nell'area Stati Uniti centro-settentrionali e il database secondario nell'area Stati Uniti centro-meridionali.

![Relazione di replica geografica](./media/sql-database-active-geo-replication/geo-replication-relationship.png)

Un altro vantaggio importante riguarda i database secondari, che sono leggibili e possono essere usati per l'offload dei carichi di lavoro di sola lettura, ad esempio i processi di creazione di report. Se si intende usare il database secondario solo per il bilanciamento del carico è possibile crearlo nella stessa area del database primario. Questo tuttavia non aumenta la resilienza dell'applicazione in caso di errori irreversibili.  

Di seguito sono illustrati altri scenari in cui è possibile usare la Replica geografica attiva:

* **Migrazione di un database**: è possibile usare la Replica geografica attiva per eseguire la migrazione di un database da un server a un altro online con un tempo di inattività minimo.
* **Aggiornamenti dell'applicazione**: è possibile creare un database secondario aggiuntivo come copia di failback durante gli aggiornamenti dell'applicazione.

Per ottenere una reale continuità aziendale, l'aggiunta di ridondanza dei database tra i data center rappresenta solo una parte della soluzione. Per ripristinare un'applicazione (servizio) end-to-end dopo un problema grave, è necessario effettuare il ripristino di tutti i componenti del servizio e gli eventuali servizi dipendenti. Esempi di questi componenti includono il software client (ad esempio, un browser con JavaScript personalizzato), front-end Web, spazio di archiviazione e DNS. È fondamentale che tutti i componenti siano resilienti agli stessi problemi e diventino disponibili entro I'obiettivo del tempo di ripristino (RTO) dell'applicazione. È perciò necessario identificare tutti i servizi dipendenti e comprendere quali garanzie e funzionalità vengono fornite. È quindi necessario intraprendere le azioni appropriate per assicurare il funzionamento del servizio durante il failover dei servizi da cui dipende. Per altre informazioni sulla progettazione di soluzioni per il ripristino di emergenza, vedere [Progettazione di soluzioni cloud per il ripristino di emergenza mediante la Replica geografica attiva](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="active-geo-replication-capabilities"></a>Funzionalità della replica geografica attiva
La funzionalità di Replica geografica attiva fornisce i seguenti elementi essenziali:

* **Replica asincrona automatica**: è possibile creare un database secondario solo aggiungendolo a un database esistente. Il database secondario può essere creato solo in un server di database SQL di Azure diverso. Dopo avere creato il database secondario, è necessario popolarlo con i dati copiati dal database primario. Questo processo è noto come seeding. Dopo aver creato ed eseguito il seeding del database secondario, gli aggiornamenti al database primario vengono replicati automaticamente in modo asincrono nel database secondario. Replica asincrona significa che le transazioni vengono sottoposte a commit nel database primario prima di essere replicate nel database secondario. 
* **Più database secondari**: due o più database secondari aumentano la ridondanza e il livello di protezione per il database primario e l'applicazione. Se sono presenti più database secondari, l'applicazione resta protetta anche se uno dei database secondari non funziona. Se è presente un solo database secondario e questo smette di funzionare, l'applicazione rimane esposta a maggiori rischi finché non viene creato un nuovo database secondario.
* **Database secondari accessibili in lettura**: un'applicazione può accedere a un database secondario per le operazioni di sola lettura usando le stesse entità di sicurezza usate per l'accesso al database primario. I database secondari operano in modalità di isolamento dello snapshot per garantire che la replica degli aggiornamenti del database primario (riesecuzione del log) non venga ritardata da query eseguite sul database secondario.

> [!NOTE]
> La riesecuzione del log viene ritardata nel database secondario se è in corso la ricezione di aggiornamenti dello schema dal database primario, perché richiede un blocco dello schema nel database secondario.
> 
> 

* **Replica geografica attiva dei database del pool elastico**: la replica geografica attiva può essere configurata per qualsiasi database in qualsiasi pool elastico. Il database secondario può trovarsi in un altro pool elastico. Per i database normali, il database secondario può essere un pool elastico e viceversa, purché i livelli di servizio restino invariati. 
* **Livello di prestazioni configurabile del database secondario**: un database secondario può essere creato con un livello di prestazioni inferiore rispetto al database primario. I database primari e secondari devono avere lo stesso livello di servizio. Questa opzione non è consigliata per le applicazioni con attività di scrittura nel database elevate perché il maggiore intervallo di replica aumenta il rischio di perdita di dati sostanziali dopo il failover. Inoltre, dopo il failover ci saranno delle ripercussioni sulle prestazioni dell'applicazione fino a quando il nuovo database primario non verrà aggiornato a un livello di prestazioni superiore. Il grafico della percentuale IO del log nel portale di Azure fornisce un buon metodo per stimare il livello di prestazioni minimo del database secondario che deve sostenere il carico della replica. Ad esempio, se il database primario è P6 (1000 DTU) e la percentuale IO del log è del 50%, il database secondario deve essere almeno P4 (500 DTU). È anche possibile recuperare i dati di I/O del log usando la vista di database [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) o [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx).  Per altre informazioni sui livelli di prestazioni del database SQL, vedere [Opzioni e prestazioni disponibili in ogni livello di servizio del database SQL](sql-database-service-tiers.md). 
* **Failover e failback controllati dall'utente**: un database secondario può essere impostato esplicitamente sul ruolo di database primario in qualsiasi momento dall'applicazione o dall'utente. Durante un'interruzione reale, deve essere usata l'opzione "non pianificato", che alza immediatamente il livello di un database secondario a primario. Quando il database primario viene ripristinato ed è nuovamente disponibile, il sistema lo contrassegna automaticamente come database secondario e lo aggiorna con il nuovo database primario. A causa della natura asincrona della replica, una piccola quantità di dati può andare perduta durante i failover non pianificati se il database primario si interrompe prima di replicare le modifiche più recenti al database secondario. Quando un database primario con più database secondari esegue il failover, il sistema riconfigura automaticamente le relazioni di replica e collega i database secondari rimanenti al nuovo database primario appena alzato di livello senza alcun intervento da parte dell'utente. Dopo aver risolto l'interruzione del servizio che ha causato il failover, è opportuno ripristinare l'applicazione nell'area primaria. A tale scopo, richiamare il comando di failover con l'opzione "pianificato". 
* **Mantenere sincronizzate le credenziali e le regole del firewall**: per i database con replica geografica è consigliabile usare le [regole del firewall a livello di database](sql-database-firewall-configure.md), in modo che tali regole possano essere replicate con il database, per garantire che tutti i database secondari abbiano le stesse regole del firewall del database primario. In questo modo non è più necessario configurare e gestire le regole del firewall manualmente nei server che ospitano sia il database primario che i secondari. Analogamente, l'accesso ai dati come [utenti di database indipendente](sql-database-manage-logins.md) fa sì che i database primari e secondari abbiano sempre le stesse credenziali utente. Durante un failover, quindi, non si verificano interruzioni dovute a una mancata corrispondenza tra account di accesso e password. Con l'aggiunta di [Azure Active Directory](../active-directory/active-directory-whatis.md) i clienti possono gestire l'accesso utente ai database primari e secondari, eliminando completamente la necessità di gestire le credenziali nei database.

## <a name="upgrading-or-downgrading-a-primary-database"></a>Aggiornamento o downgrade di un database primario
È possibile eseguire l'aggiornamento o il downgrade di un database primario a un livello di prestazioni diverso entro lo stesso livello di servizio, senza disconnettere eventuali database secondari. Quando si esegue l'aggiornamento, è consigliabile aggiornare prima di tutto il database secondario e quindi il database primario. Quando si esegue il downgrade, seguire l'ordine inverso, ovvero eseguire prima di tutto il downgrade del database primario e quindi del database secondario. 

Il database secondario deve trovarsi nello stesso livello di servizio del database primario. Per eseguire la migrazione del database primario a un livello di servizio diverso, è quindi necessario interrompere il collegamento di replica geografica e rinominare il database secondario oppure semplicemente rilasciarlo. Eseguire quindi la migrazione del database primario al nuovo livello di servizio e riconfigurare la replica geografica. Il nuovo database secondario verrà creato automaticamente con lo stesso livello di prestazioni del database primario, per impostazione predefinita.

## <a name="preventing-the-loss-of-critical-data"></a>Evitare la perdita di dati critici
A causa della latenza elevata delle reti WAN, per la copia continua viene usato un meccanismo di replica asincrona. La replica asincrona rende inevitabile una perdita parziale dei dati nel caso si verifichi un problema. Tuttavia, alcune applicazione potrebbero non essere soggette alla perdita dei dati. Per proteggere questi aggiornamenti critici, uno sviluppatore di applicazioni può chiamare la procedura di sistema [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx) subito dopo il commit della transazione. La chiamata a **sp_wait_for_database_copy_sync** blocca il thread chiamante finché non viene completata la replica dell'ultima transazione di cui è stato eseguito il commit nel database secondario. La procedura resta in attesa finché tutte le transazioni in coda non vengono riconosciute dal database secondario. **sp_wait_for_database_copy_sync** ha come ambito un collegamento di copia continua specifico. La procedura può essere chiamata da qualsiasi utente che abbia diritti di connessione al database primario.

> [!NOTE]
> Il ritardo causato da una chiamata alla procedura **sp_wait_for_database_copy_sync** può essere considerevole. Il ritardo dipende dalle dimensioni della lunghezza del log delle transazioni corrente e non termina finché non viene replicato l'intero log. Chiamare quindi la procedura solo quando strettamente necessario.
> 
> 

## <a name="programmatically-managing-active-geo-replication"></a>Gestione a livello di codice della replica geografica attiva
Come indicato in precedenza, la replica geografica attiva può essere gestita a livello di codice usando Azure PowerShell e l'API REST. Le tabelle seguenti descrivono il set di comandi disponibili.

* **API di Azure Resource Manager e sicurezza basata sui ruoli**: la replica geografica attiva include un set di [API di Azure Resource Manager](https://msdn.microsoft.com/library/azure/mt163571.aspx) per la gestione, inclusi i [cmdlet di PowerShell basati su Azure Resource Manager](scripts/sql-database-setup-geodr-and-failover-database-powershell.md). Queste API richiedono l'uso di gruppi di risorse e supportano la sicurezza basata sui ruoli (Controllo degli accessi in base al ruolo). Per altre informazioni su come implementare i ruoli di accesso, vedere [Controllo degli accessi in base al ruolo di Azure](../active-directory/role-based-access-control-configure.md).

> [!NOTE]
> Molte nuove funzionalità della replica geografica attiva sono supportate solo con [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) e in particolare con le [API REST SQL di Azure](https://msdn.microsoft.com/library/azure/mt163571.aspx) e con i [cmdlet di PowerShell per il database SQL di Azure](https://msdn.microsoft.com/library/azure/mt574084.aspx). L'[API REST (versione classica)](https://msdn.microsoft.com/library/azure/dn505719.aspx) e i [cmdlet del database SQL di Azure (versione classica)](https://msdn.microsoft.com/library/azure/dn546723.aspx) sono supportati per la compatibilità con le versioni precedenti, pertanto è consigliabile usare le API basate su Azure Resource Manager. 
> 
> 

### <a name="transact-sql"></a>Transact-SQL
| Comando | Descrizione |
| --- | --- |
| [ALTER DATABASE (database SQL di Azure)](https://msdn.microsoft.com/library/mt574871.aspx) |Usare l'argomento ADD SECONDARY ON SERVER per creare un database secondario per un database esistente e avviare la replica dei dati |
| [ALTER DATABASE (database SQL di Azure)](https://msdn.microsoft.com/library/mt574871.aspx) |Usare FAILOVER o FORCE_FAILOVER_ALLOW_DATA_LOSS per passare un database secondario al ruolo di database primario per avviare il failover |
| [ALTER DATABASE (database SQL di Azure)](https://msdn.microsoft.com/library/mt574871.aspx) |Usare REMOVE SECONDARY ON SERVER per terminare la replica dei dati tra un database SQL e il database secondario specificato. |
| [sys.geo_replication_links (database SQL di Azure)](https://msdn.microsoft.com/library/mt575501.aspx) |Restituisce informazioni su tutti i collegamenti di replica esistenti per ogni database nel server logico del database SQL di Azure. |
| [sys.dm_geo_replication_link_status (database SQL di Azure)](https://msdn.microsoft.com/library/mt575504.aspx) |Ottiene l'ultima ora di replica, l'ultimo intervallo di replica e altre informazioni sul collegamento di replica per un database SQL specificato. |
| [sys.dm_operation_status (database SQL di Azure)](https://msdn.microsoft.com/library/dn270022.aspx) |Mostra lo stato per tutte le operazioni di database, incluso lo stato dei collegamenti di replica. |
| [sp_wait_for_database_copy_sync (database SQL di Azure)](https://msdn.microsoft.com/library/dn467644.aspx) |fa sì che l'applicazione rimanga in attesa finché tutte le transazioni vengono replicate e riconosciute dal database secondario attivo. |
|  | |

### <a name="powershell"></a>PowerShell
| Cmdlet | Descrizione |
| --- | --- |
| [Get-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt603648.aspx) |Ottiene uno o più database. |
| [New-AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603689.aspx) |Crea un database secondario per un database esistente e avvia la replica dei dati. |
| [Set-AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt619393.aspx) |Consente di passare un database secondario al ruolo di database primario per avviare il failover. |
| [Remove-AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603457.aspx) |Termina la replica dei dati tra un database SQL e il database secondario specificato. |
| [Get-AzureRmSqlDatabaseReplicationLink](https://msdn.microsoft.com/library/mt619330.aspx) |Ottiene i collegamenti di replica geografica tra un database di SQL Azure e un gruppo di risorse o SQL Server. |
|  | |

### <a name="rest-api"></a>API REST
| API | Descrizione |
| --- | --- |
| [Creare o aggiornare database (createMode=Restore)](https://msdn.microsoft.com/library/azure/mt163685.aspx) |Crea, aggiorna o ripristina un database primario o secondario. |
| [Get Create or Update Database Status](https://msdn.microsoft.com/library/azure/mt643934.aspx) |Restituisce lo stato durante un'operazione di creazione. |
| [Impostazione del database secondario come primario (failover pianificato)](https://msdn.microsoft.com/library/azure/mt575007.aspx) |Innalza di livello un database secondario in una relazione di replica geografica in modo che diventi il nuovo database primario. |
| [Set Secondary Database as Primary (Unplanned Failover)](https://msdn.microsoft.com/library/azure/mt582027.aspx) |Per forzare un failover nel database secondario e impostare il database secondario come database primario. |
| [Get Replication Links](https://msdn.microsoft.com/library/azure/mt600929.aspx) |Ottiene tutti i collegamenti di replica per un database SQL specificato in una relazione di replica geografica. Recupera le informazioni visibili nella vista del catalogo sys.geo_replication_links. |
| [Get Replication Link](https://msdn.microsoft.com/library/azure/mt600778.aspx) |Ottiene tutti i collegamenti di replica specifici per un database SQL specificato in una relazione di replica geografica. Recupera le informazioni visibili nella vista del catalogo sys.geo_replication_links. |
|  | |

## <a name="next-steps"></a>Passaggi successivi
* Per la panoramica e gli scenari della continuità aziendale, vedere [Continuità aziendale del database SQL di Azure](sql-database-business-continuity.md)
* Per informazioni sui backup automatici del database SQL di Azure, vedere [Backup automatici del database SQL](sql-database-automated-backups.md).
* Per altre informazioni sull'uso dei backup automatici per il ripristino, vedere l'articolo relativo al [ripristino di un database dai backup avviati dal servizio](sql-database-recovery-using-backups.md).
* Per altre informazioni sull'uso dei backup automatici per l'archiviazione, vedere [Copiare un database SQL di Azure](sql-database-copy.md).
* Per ulteriori informazioni sui requisiti di autenticazione per un nuovo database e server primario, vedere l'articolo sulla [sicurezza del database SQL di Azure dopo il ripristino di emergenza](sql-database-geo-replication-security-config.md).


