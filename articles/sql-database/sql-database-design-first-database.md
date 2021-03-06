---
title: Progettare il primo database SQL di Azure | Microsoft Docs
description: Informazioni su come progettare il primo database SQL di Azure.
services: sql-database
documentationcenter: 
author: janeng
manager: jstrauss
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: tutorial-develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 03/30/2017
ms.author: janeng
translationtype: Human Translation
ms.sourcegitcommit: 2c33e75a7d2cb28f8dc6b314e663a530b7b7fdb4
ms.openlocfilehash: 0d02954829ebac9275c014f7dac7e1ec423b0fc1
ms.lasthandoff: 04/21/2017


---

# <a name="design-your-first-azure-sql-database"></a>Progettare il primo database SQL di Azure

In questa esercitazione si compila un database per permettere a un'università di tenere traccia dei corsi e dei livelli di registrazione degli studenti. In questa esercitazione viene illustrato come usare il [Portale di Azure](https://portal.azure.com/) e [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS) per creare un database SQL di Azure in un server logico del Database SQL di Azure, aggiungere tabelle al database, caricare i dati nelle tabelle ed eseguire query sul database. Viene anche illustrato come usare le funzionalità [ripristino temporizzato](sql-database-recovery-using-backups.md#point-in-time-restore) del Database SQL per ripristinare il database a un punto precedente nel tempo.

Per completare questa esercitazione, assicurarsi di aver installato la versione più recente di [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS). 

## <a name="log-in-to-the-azure-portal"></a>Accedere al Portale di Azure.

Accedere al [Portale di Azure](https://portal.azure.com/).

## <a name="create-a-blank-sql-database-in-azure"></a>Creare un database SQL vuoto in Azure

Un database SQL di Azure viene creato con un set definito di [risorse di calcolo e di archiviazione](sql-database-service-tiers.md). Il database viene creato in un [gruppo di risorse di Azure](../azure-resource-manager/resource-group-overview.md) e in un [server logico di database SQL di Azure](sql-database-features.md). 

Per creare un database SQL vuoto, attenersi alla procedura seguente. 

1. Fare clic sul pulsante **Nuovo** nell'angolo superiore sinistro del portale di Azure.

2. Selezionare **Database** nella pagina **Nuovo** e quindi **Database SQL** nella pagina **Database**. 

    ![creare database vuoto](./media/sql-database-design-first-database/create-empty-database.png)

3. Compilare il modulo Database SQL con le informazioni seguenti, come illustrato nell'immagine precedente:     

   - Nome database: **mySampleDatabase**
   - Gruppo di risorse: **myResourceGroup**
   - Origine: **database vuoto**

4. Fare clic su **Server** per creare e configurare un nuovo server per il nuovo database. Compilare il **modulo Nuovo server** specificando un nome server univoco globale, immettere un nome per l'accesso amministratore server e quindi specificare la password scelta. 

    ![Creare il server di database](./media//sql-database-design-first-database/create-database-server.png)
5. Fare clic su **Seleziona**.

6. Fare clic su **Piano tariffario** per specificare il livello di servizio e il livello delle prestazioni per il nuovo database. Per esercitazione selezionare **20 DTU** e **250** GB di memoria.

    ![Creare il database s1](./media/sql-database-design-first-database/create-empty-database-pricing-tier.png)

7. Fare clic su **Apply**.  

8. Fare clic su **Crea** per effettuare il provisioning del database. Il provisioning richiede circa un minuto e mezzo per il completamento. 

9. Sulla barra degli strumenti fare clic su **Notifiche** per monitorare il processo di distribuzione.

    ![notifica](./media/sql-database-get-started-portal/notification.png)


## <a name="create-a-server-level-firewall-rule"></a>Creare una regola del firewall a livello di server

I database SQL di Azure sono protetti da un firewall. Per impostazione predefinita, vengono rifiutate tutte le connessioni al server e ai database all'interno del server. Seguire questa procedura per creare una [regola del firewall a livello di server del Database SQL](sql-database-firewall-configure.md) per il server per consentire le connessioni dall'indirizzo IP del client. 

1. Al termine della distribuzione, scegliere **Database SQL** dal menu a sinistra e fare clic sul nuovo database, **mySampleDatabase**, nella pagina **Database SQL**. Si apre la pagina di panoramica per il database che visualizza il nome completo del server, ad esempio **mynewserver20170313.database.windows.net**, e offre altre opzioni di configurazione.

      ![Regola del firewall del server](./media/sql-database-design-first-database/server-firewall-rule.png) 

2. Fare clic su **Imposta firewall server** sulla barra degli strumenti, come illustrato nell'immagine precedente. Si apre la pagina **Impostazioni del firewall** per il server del database SQL. 

3. Fare clic su **Aggiungi IP client** sulla barra degli strumenti e fare clic su **Salva**. Una regola del firewall a livello di server viene creata per l'indirizzo IP corrente.

      ![Impostare la regola del firewall del server](./media/sql-database-design-first-database/server-firewall-rule-set.png) 

4. Fare clic su **OK** e quindi sulla **X** per chiudere la pagina **Impostazioni del firewall**.

È ora possibile connettersi al database e al server usando SQL Server Management Studio o un altro strumento di propria scelta.

> [!NOTE]
> Il database SQL comunica attraverso la porta 1433. Se si sta tentando di connettersi da una rete aziendale, il traffico in uscita attraverso la porta 1433 potrebbe non essere autorizzato dal firewall della rete. In questo caso, non sarà possibile connettersi al server del database SQL di Azure, a meno che il reparto IT non apra la porta 1433.
>

## <a name="get-connection-information"></a>Ottenere informazioni di connessione

Ottenere il nome completo del server per il server del database SQL di Azure nel portale di Azure. Usare il nome completo del server per connettersi al server tramite SQL Server Management Studio.

1. Accedere al [Portale di Azure](https://portal.azure.com/).
2. Scegliere **Database SQL** dal menu a sinistra, quindi fare clic sul database nella pagina **Database SQL**. 
3. Nel riquadro **Informazioni di base** della pagina del portale di Azure per il database individuare e quindi copiare il **Nome server**.

    ![informazioni di connessione](./media/sql-database-connect-query-ssms/connection-information.png) 

## <a name="connect-to-your-database-using-sql-server-management-studio"></a>Connettersi al database tramite SQL Server Management Studio

Usare [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) per stabilire una connessione al server del database SQL di Azure.

1. Aprire SQL Server Management Studio.

2. Nella finestra di dialogo **Connetti al server** immettere le informazioni seguenti:
   - **Tipo di server**: specificare il motore del database
   - **Nome server**: immettere il nome completo del server, ad esempio **mynewserver20170313.database.windows.net**
   - **Autenticazione**: specificare l'Autenticazione di SQL Server
   - **Accesso**: immettere l'account dell'amministratore del server
   - **Password**: immettere la password per l'account dell'amministratore del server


   <img src="./media/sql-database-connect-query-ssms/connect.png" alt="connect to server" style="width: 780px;" />

3. Fare clic su **Opzioni** nella finestra di dialogo **Connetti al server**. Nella sezione **Connetti al database** immettere **mySampleDatabase** per connettersi a tale database.

   ![connettersi al database nel server](./media/sql-database-connect-query-ssms/options-connect-to-db.png)  

4. Fare clic su **Connetti**. La finestra Esplora oggetti viene visualizzata in SSMS. 

5. In Esplora oggetti espandere **Database** e quindi espandere **mySampleDatabase** per visualizzare gli oggetti disponibili nel database di esempio.

   ![oggetti di database](./media/sql-database-connect-query-ssms/connected.png)  

## <a name="create-tables-in-the-database"></a>Creare tabelle nel database 

Creare uno schema di database con quattro tabelle che modellano un sistema di gestione degli studenti per le università tramite [Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference):

- Person
- Corso
- Studente
- Accreditare quel modello come un sistema di gestione degli studenti per le università

Nel diagramma seguente viene illustrato come queste tabelle sono correlate tra loro. Alcune di queste tabelle fanno riferimento a delle colonne in altre tabelle. Ad esempio, la tabella Student fa riferimento alla colonna **PersonId** della tabella **Person**. Studiare il diagramma per comprendere come sono correlate tra loro le tabelle in questa esercitazione. Per un esame approfondito su come creare tabelle di database efficaci, vedere [Create effective database tables](https://msdn.microsoft.com/library/cc505842.aspx) (Creare tabelle di database efficaci). Per informazioni sulla scelta dei tipi di dati, vedere [Tipi di dati](https://docs.microsoft.com/sql/t-sql/data-types/data-types-transact-sql).

> [!NOTE]
> È anche possibile usare la [Progettazione delle tabelle in SQL Server Management Studio](https://msdn.microsoft.com/library/hh272695.aspx) per creare e progettare le tabelle. 

![Relazioni tra tabelle](./media/sql-database-design-first-database/tutorial-database-tables.png)

1. In Esplora oggetti fare clic con il pulsante destro del mouse su **mySampleDatabase** e scegliere **Nuova query**. Viene visualizzata una finestra di query vuota, connessa al database.

2. Nella finestra di query, eseguire la query seguente per creare quattro tabelle nel database: 

   ```sql 
   -- Create Person table

    CREATE TABLE Person
    (
      PersonId      INT IDENTITY PRIMARY KEY,
      FirstName     NVARCHAR(128) NOT NULL,
      MiddelInitial NVARCHAR(10),
      LastName      NVARCHAR(128) NOT NULL,
      DateOfBirth   DATE NOT NULL
    )
   
   -- Create Student table
 
    CREATE TABLE Student
    (
      StudentId INT IDENTITY PRIMARY KEY,
      PersonId  INT REFERENCES Person (PersonId),
      Email     NVARCHAR(256)
    )
    
   -- Create Course table
 
    CREATE TABLE Course
    (
      CourseId  INT IDENTITY PRIMARY KEY,
      Name      NVARCHAR(50) NOT NULL,
      Teacher   NVARCHAR(256) NOT NULL
    ) 

   -- Create Credit table
 
    CREATE TABLE Credit
    (
      StudentId   INT REFERENCES Student (StudentId),
      CourseId    INT REFERENCES Course (CourseId),
      Grade       DECIMAL(5,2) CHECK (Grade <= 100.00),
      Attempt     TINYINT,
      CONSTRAINT  [UQ_studentgrades] UNIQUE CLUSTERED
      (
        StudentId, CourseId, Grade, Attempt
      )
    )
   ```

![Creare tabelle](./media/sql-database-design-first-database/create-tables.png)

3. Espandere il nodo "tabelle" in SQL Server Management Studio Object Explorer per visualizzare le tabelle create.

   ![tabelle create con SQL Server Management Studio](./media/sql-database-design-first-database/ssms-tables-created.png)

## <a name="load-data-into-the-tables"></a>Caricare i dati nelle tabelle

1. Creare una cartella denominata **SampleTableData** nella cartella download per archiviare i dati di esempio per il database. 

2. Fare doppio clic sui seguenti collegamenti e salvarli nella cartella **SampleTableData**. 

   - [SampleCourseData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleCourseData)
   - [SamplePersonData](https://sqldbtutorial.blob.core.windows.net/tutorials/SamplePersonData)
   - [SampleStudentData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleStudentData)
   - [SampleCreditData](https://sqldbtutorial.blob.core.windows.net/tutorials/SampleCreditData)

3. Aprire una finestra del prompt dei comandi e passare alla cartella SampleTableData.

4. Eseguire i comandi seguenti per inserire dei dati di esempio nelle tabelle sostituendo i valori per **ServerName**, **DatabaseName**, **UserName** e **Password** con i valori dell'ambiente.
  
   ```bcp
   bcp Course in SampleCourseData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   bcp Person in SamplePersonData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   bcp Student in SampleStudentData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   bcp Credit in SampleCreditData -S <ServerName>.database.windows.net -d <DatabaseName> -U <Username> -P <password> -q -c -t ","
   ```

A questo punto, sono stati caricati i dati di esempio nelle tabelle create in precedenza.

## <a name="query-the-tables"></a>Eseguire una query nelle tabelle

Eseguire le query seguenti per recuperare informazioni dalle tabelle del database. Vedere [Writing SQL Queries](https://technet.microsoft.com/library/bb264565.aspx) (Scrittura di query SQL) per altre informazioni sulla scrittura di query SQL. La prima query unisce tutte e quattro le tabelle per trovare tutti gli studenti a cui ha insegnato "Dominick Pope" che hanno un livello superiore al 75% nella sua classe. La seconda query unisce tutte e quattro le tabelle e trova tutti i corsi in cui si è registrato "Noe Coleman".

1. In una finestra di query di SQL Server Management Studio, eseguire la query seguente:

   ```sql 
   -- Find the students taught by Dominick Pope who have a grade higher than 75%

    SELECT  person.FirstName,
        person.LastName,
        course.Name,
        credit.Grade
    FROM  Person AS person
        INNER JOIN Student AS student ON person.PersonId = student.PersonId
        INNER JOIN Credit AS credit ON student.StudentId = credit.StudentId
        INNER JOIN Course AS course ON credit.CourseId = course.courseId
    WHERE course.Teacher = 'Dominick Pope' 
        AND Grade > 75
   ```

2. In una finestra di query di SQL Server Management Studio, eseguire la query seguente:

   ```sql
   -- Find all the courses in which Noe Coleman has ever enrolled

    SELECT  course.Name,
        course.Teacher,
        credit.Grade
    FROM  Course AS course
        INNER JOIN Credit AS credit ON credit.CourseId = course.CourseId
        INNER JOIN Student AS student ON student.StudentId = credit.StudentId
        INNER JOIN Person AS person ON person.PersonId = student.PersonId
    WHERE person.FirstName = 'Noe'
        AND person.LastName = 'Coleman'
   ```

## <a name="restore-a-database-to-a-previous-point-in-time"></a>Ripristinare un database a un momento precedente 

Si supponga di aver eliminato accidentalmente una tabella. Si tratta di un elemento che non è facile da ripristinare. Il Database SQL di Azure consente di tornare in qualsiasi punto degli ultimi 35 giorni e di ripristinare questo punto nel tempo in un nuovo database. È possibile usare questo database per ripristinare i dati eliminati. La procedura seguente consente di ripristinare il database di esempio in un punto precedente all'aggiunta delle tabelle.

1. Nella pagina Database SQL del database fare clic su **Ripristina** sulla barra degli strumenti. Si apre la pagina **Ripristina** .

   ![ripristinare](./media/sql-database-design-first-database/restore.png)

2. Compilare il modulo **Ripristina** con le informazioni obbligatorie:
    * Nome database: specificare un nome per il database 
    * Temporizzato: Selezionare la scheda **Temporizzato** del modulo Ripristino 
    * Punto di ripristino: selezionare un punto nel tempo precedente alla modifica del database
    * Server di destinazione: non è possibile modificare questo valore quando si ripristina un database 
    * Pool di database elastici: selezionare **Nessuno**  
    * Piano tariffario: selezionare **20 DTUs** (20 DTU) e **250 GB** di memoria.

   ![punto di ripristino](./media/sql-database-design-first-database/restore-point.png)

3. Fare clic su **OK** per ripristinare il database da ripristinare [ in un punto nel tempo ](sql-database-recovery-using-backups.md#point-in-time-restore) precedente all'aggiunta delle tabelle. Il ripristino di un database in un altro punto nel tempo crea un duplicato del database nello stesso server del database originale nel punto nel tempo specificato, purché sia entro il periodo di conservazione per il [livello di servizio](sql-database-service-tiers.md) applicato.

## <a name="next-steps"></a>Passaggi successivi 

Per esempi di attività comuni di PowerShell, vedere gli [esempi di PowerShell per Database SQL](sql-database-powershell-samples.md)

