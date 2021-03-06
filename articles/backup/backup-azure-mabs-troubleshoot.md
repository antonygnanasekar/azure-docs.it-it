---
title: Risolvere i problemi del server di Backup di Azure | Microsoft Docs
description: Risolvere i problemi di installazione e registrazione del server di Backup di Azure ed eseguire backup e ripristino dei carichi di lavoro delle applicazioni
services: backup
documentationcenter: 
author: pvrk
manager: shreeshd
editor: 
ms.assetid: 2d73c349-0fc8-4ca8-afd8-8c9029cb8524
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/24/2017
ms.author: pullabhk;markgal;
translationtype: Human Translation
ms.sourcegitcommit: 356de369ec5409e8e6e51a286a20af70a9420193
ms.openlocfilehash: a42488d618c58b36fa8105c1b22fd32ca615d1b1
ms.lasthandoff: 03/27/2017


---

# <a name="troubleshoot-azure-backup-server"></a>Risolvere i problemi del server di Backup di Azure

È possibile risolvere gli errori rilevati durante l'uso di server di Backup di Azure con le informazioni elencate nella tabella seguente.

>
>

## <a name="registration-and-agent-related-issues"></a>Problemi relativi alla registrazione e all'agente
| Operazione | Dettagli errore | Soluzione alternativa |
| --- | --- | --- |
| Registrazione in un insieme di credenziali | Sono state specificate credenziali di insieme non valide. Il file è danneggiato o non ha le credenziali più recenti associate al servizio di ripristino | Per correggere l'errore:  <ol><li> Scaricare il file di credenziali più recente dall'insieme e riprovare <br>OPPURE</li> <li> Se il problema non viene risolto, provare a scaricare le credenziali in una directory locale diversa o creare un nuovo insieme <br>OPPURE</li> <li> Provare ad aggiornare le impostazioni di data e ora come illustrato in [questo blog](https://azure.microsoft.com/blog/troubleshooting-common-configuration-issues-with-azure-backup/) <br>OPPURE</li> <li> Controllare se il percorso c:\windows\temp contenga più di 65.000 file. Spostare i file non aggiornati in un altro percorso o eliminare gli elementi nella cartella Temp <br>OPPURE</li> <li> Controllare lo stato dei certificati <br> a. Aprire "Gestisci i certificati computer" nel Pannello di controllo <br> b. Espandere il nodo "Personale" e il relativo nodo figlio "Certificati" <br> c.  Rimuovere il certificato "Strumenti di Windows Azure" <br> d. Ripetere la registrazione nel client di Backup di Azure <br> OPPURE </li> <li> Verificare se siano applicati eventuali criteri di gruppo </li></ol> |
| Push degli agenti in server protetti | Operazione agente non riuscita a causa di un errore di comunicazione con il servizio Coordinatore agenti DPM in \<ServerName> | **Se l'azione consigliata nel prodotto non funziona**, <ol><li> Se si collega un computer da un dominio non trusted, seguire [questi](https://technet.microsoft.com/library/hh757801(v=sc.12).aspx) passaggi <br> OPPURE </li><li> Se si collega un computer da un dominio trusted, risolvere i problemi mediante i passaggi descritti in [questo post di blog](https://blogs.technet.microsoft.com/dpm/2012/02/06/data-protection-manager-agent-network-troubleshooting/) <br>OPPURE</li><li> Provare a disabilitare l'antivirus come passaggio per la risoluzione dei problemi. Se il problema viene risolto, modificare le impostazioni dell'antivirus come suggerito in [questo articolo] (https://technet.microsoft.com/library/hh757911.aspx)</li></ol> |
| Push degli agenti in server protetti | Le credenziali specificate per il server non sono valide | **Se l'azione consigliata nel prodotto non funziona**, <br> provare a installare manualmente l'agente protezione nel server di produzione come specificato in [questo articolo](https://technet.microsoft.com/library/hh758186(v=sc.12).aspx#BKMK_Manual)|


## <a name="configuring-protection-group"></a>Configurazione di un gruppo protezione dati
| Operazione | Dettagli errore | Soluzione alternativa |
| --- | --- | --- |
| Configurazione di gruppi di protezione | DPM non è in grado di enumerare il componente dell'applicazione nel computer protetto (Nome computer protetto) | Fare clic su "Aggiorna"' nella schermata dell'interfaccia utente per la configurazione del gruppo protezione dati al livello di origine dati/componente appropriato |
| Configurazione di gruppi di protezione | Impossibile configurare la protezione | Se il server protetto è un server SQL, verificare se le autorizzazioni del ruolo sysadmin siano state fornite all'account di sistema (NTAuthority\System) nel computer protetto come indicato in [questo articolo](https://technet.microsoft.com/library/hh757977(v=sc.12).aspx)
| Configurazione di gruppi di protezione | Non c'è spazio sufficiente nel pool di archiviazione per questo gruppo protezione dati | I dischi aggiunti al pool di archiviazione [non devono contenere una partizione](https://technet.microsoft.com/library/hh758075(v=sc.12).aspx). Eliminare i volumi presenti nei dischi e quindi aggiungerlo al pool di archiviazione|

## <a name="backup"></a>Backup
| Operazione | Dettagli errore | Soluzione alternativa |
| --- | --- | --- |
| Backup | La replica è incoerente | Altri dettagli sulle cause di incoerenza nella replica e relativi suggerimenti sono disponibili [qui](https://technet.microsoft.com/library/cc161593.aspx) <br> <ol><li> In caso di backup dello stato del sistema o di ripristino bare metal, verificare se Windows Server Backup sia installato o meno nel server protetto </li><li> Verificare la presenza di problemi di spazio nel pool di archiviazione DPM nel server DPM/MABS e allocare ulteriore memoria in base alle necessità </li><li> Controllare lo stato del servizio Copia Shadow del volume nel server protetto. Se è disabilitato, impostarlo per l'avvio manuale e avviare il servizio nel server. Tornare quindi alla console di DPM/MABS e avviare la sincronizzazione con verifica della coerenza.</li></ol>|
| Backup | Si è verificato un errore imprevisto durante l'esecuzione del processo, il dispositivo non è pronto | **Se l'azione consigliata nel prodotto non funziona**, <br> <ol><li>impostare lo spazio di archiviazione della Copia Shadow come illimitato negli elementi del gruppo protezione dati e avviare la verifica coerenza <br></li> OPPURE <li>Provare a eliminare il gruppo protezione dati esistente e crearne di nuovi, ciascuno contenente ogni elemento singolo</li></ol> |
| Backup | Se si sta eseguendo il backup solamente dello stato del sistema, verificare che ci sia spazio libero sufficiente nel computer protetto per archiviarlo | <ol><li>Verificare che WSB sia installato nel computer protetto</li><li>Verificare che ci sia spazio sufficiente nel computer protetto per lo stato del sistema: il modo più semplice per farlo è aprire WSB nel computer protetto, fare clic nelle varie selezioni e selezionare il ripristino bare metal. A quel punto, l'interfaccia utente indicherà lo spazio richiesto per l'operazione. Aprire WSB -> Backup locale -> Pianificazione backup -> Selezione configurazione di backup -> Server completo (vengono visualizzate le dimensioni). Usare queste dimensioni per la verifica.</li></ol>
| Backup | Impossibile creare il punto di ripristino online | Se viene visualizzato il messaggio di errore "L'agente di Backup di Microsoft Azure non è riuscito a creare uno snapshot del volume selezionato.", provare ad aumentare lo spazio nel volume del punto di ripristino e della replica.
| Backup | Impossibile creare il punto di ripristino online | Se viene visualizzato il messaggio di errore "Non è stata impostata la passphrase di crittografia per questo server. Configurare una passphrase di crittografia.", provare a configurare una passphrase di crittografia. In caso di esito negativo, <br> <ol><li>verificare se esista o meno il percorso dei file temporanei. Dovrebbe esistere il percorso indicato nel Registro di sistema HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Azure Backup\Config con nome "ScratchLocation".</li><li> Se il percorso dei file temporanei esiste, provare nuovamente a eseguire la registrazione con la passphrase precedente. **Quando si configura una passphrase di crittografia, salvarla in un luogo sicuro**</li><ol>
| Backup | Errore di backup per il ripristino bare metal | Se il ripristino bare metal ha dimensioni elevate, riprovare dopo aver spostato alcuni file di applicazione nell'unità del sistema operativo |
| Backup | Errore durante l'accesso a file o cartelle condivise | Provare a modificare le impostazioni dell'antivirus come suggerito [qui](https://technet.microsoft.com/library/hh757911.aspx)|

