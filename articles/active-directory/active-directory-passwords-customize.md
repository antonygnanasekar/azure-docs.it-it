---
title: 'Personalizzazione: reimpostazione password self-service di Azure AD | Documentazione Microsoft'
description: 
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: joflore
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: 41da786ed13308a1fc030e6b02fac3d8fbca9e61
ms.contentlocale: it-it
ms.lasthandoff: 05/03/2017


---
# <a name="customize-azure-ad-functionality-for-self-service-password-reset"></a>Personalizzare la funzionalità di Azure AD per la Reimpostazione self-service delle password

Sono disponibili molte opzioni che è possibile modificare all'interno di Azure AD e molte sono correlate con la Reimpostazione self-service delle password (SSPR). Questa guida illustra quelle possibili.

## <a name="customize-the-contact-your-administrator-link"></a>Personalizzare il collegamento Contattare l'amministratore

Anche se la SSPR non è abilitata gli utenti possono ancora usare un collegamento "contattare l'amministratore" nel portale di reimpostazione della password.  Quando si fa click, questo collegamento invia un messaggio di posta elettronica agli amministratori richiedendo assistenza nella modifica della password dell'utente. Questo messaggio di posta elettronica viene inviato ai destinatari seguenti nell'ordine seguente:

1. Se viene assegnato il ruolo di **Amministratore password**, agli utenti con questo ruolo viene inviata una notifica
2. Se non è stato assegnato nessun Amministratore password, la notifica verrà inviata a coloro che hanno il ruolo di **Amministratore utente**
3. Se nessuno dei ruoli precedenti è stato assegnato, la notifica sarà inviata agli **Amministratori globali**

In tutti i casi, viene inviata una notifica a un massimo di 100 destinatari totali.

### <a name="disable-contact-your-administrator-emails"></a>Disabilitare i messaggi di posta elettronica Contattare l'amministratore

Se l'organizzazione non desidera che venga inviata una notifica agli amministratori circa le richieste di reimpostazione della password, è possibile disabilitare la seguente configurazione

* Abilitare la reimpostazione self-service delle password per tutti gli utenti finali. Questa opzione è in **Reimpostazione password**, **Proprietà**
    * Se non si desidera che gli utenti reimpostino le proprie password, è possibile definire l'ambito di accesso a un gruppo vuoto. Questa opzione non è consigliabile**.
* Personalizzare il collegamento al supporto tecnico per fornire un URL Web o mailto: indirizzo che gli utenti possono usare per ottenere assistenza. Questa opzione si trova in **Reimpostazione password**, **Personalizzazione** e quindi **Indirizzo di posta elettronica o URL del supporto tecnico**

## <a name="customize-the-sign-in-and-access-panel-look-and-feel"></a>Personalizzare l'aspetto del pannello di iscrizione e di accesso

Quando gli utenti accedono alla pagina di accesso, è possibile personalizzare il logo che compare insieme all'immagine della pagina di accesso per adattare il marchio della società.

Questi elementi grafici vengono visualizzati nelle seguenti circostanze:

* Dopo che l'utente digita il proprio nome utente
* L'utente accede a un url personalizzato
    * Passando il parametro "whr" alla pagina per la reimpostazione della password, ad esempio "https://login.microsoftonline.com/?whr=contoso.com"
    * Passando il parametro "username" alla pagina per la reimpostazione della password, ad esempio "https://login.microsoftonline.com/?username=admin@contoso.com"

### <a name="graphics-details"></a>Dettagli di grafica

Le impostazioni seguenti consentono di modificare le caratteristiche visive della pagina di accesso e possono essere trovate in **Azure Active Directory**, **Informazioni personalizzate distintive dell'azienda**, **	Modifica informazioni personalizzate distintive dell'azienda**

* L'immagine della pagina di accesso deve essere un file con estensione PNG o JPG da 1420 x 1200 pixel e non superiore a 500 KB. È consigliabile che sia di circa 200 KB per ottenere risultati ottimali.
* Il colore dello sfondo nella pagina di accesso viene usato nel caso di connessioni a latenza elevata e deve essere nel formato RGB esadecimale
* L'immagine del banner deve essere un file con estensione PNG o JPG da 60 x 280 pixel e non superiore a 10 KB
* Logo quadrato (tema normale e scuro) PNG o JPG 240 x 240 (ridimensionabile) non superiore a 10 KB

### <a name="sign-in-text-options"></a>Opzioni del testo di accesso

Le impostazioni seguenti consentono di aggiungere alla pagina di accesso del testo rilevante per l'organizzazione. Queste impostazioni sono disponibili nella sezione **Azure Active Directory**, **Informazioni personalizzate distintive dell'azienda**, **Modifica informazioni personalizzate distintive dell'azienda**

* **Suggerimento nome utente** sostituisce il testo di esempio di someone@example.com con un nome più appropriato per gli utenti, che si consiglia di mantenere come impostazione predefinita quando si supportano gli utenti interni ed esterni
* **Testo della pagina di accesso** contiene un massimo di 256 caratteri. Questo testo viene visualizzato ovunque gli utenti abbiano accesso e nell'esperienza Join di Azure AD su Windows 10. Usare questo testo per le condizioni di utilizzo, le istruzioni e i suggerimenti per gli utenti. **Chiunque può visualizzare la pagina di accesso pertanto si consiglia di non fornire informazioni riservate qui.**

### <a name="keep-me-signed-in-disabled"></a>Mantieni l'accesso disabilitata

L'opzione "Mantieni l'accesso disabilitata" consente agli utenti di restare connessi quando chiudono e riaprono la finestra del browser e non influisce sulla durata della sessione. Queste impostazioni sono disponibili nella sezione **Azure Active Directory**, **Informazioni personalizzate distintive dell'azienda**, **Modifica informazioni personalizzate distintive dell'azienda**.

Alcune funzionalità di SharePoint Online e di Office 2010 dipendono dalla possibilità per gli utenti di selezionare questa casella. Se si nasconde questa opzione, gli utenti possono ottenere prompt di accesso aggiuntivi e imprevisti.

### <a name="directory-name"></a>Nome della directory

È possibile modificare l'attributo del nome in **Azure Active Directory**, **Proprietà** per mostrare un nome descrittivo dell'organizzazione visualizzato nel portale e nelle comunicazioni automatiche. Questa opzione è più visibile sotto forma di messaggi di posta elettronica automatici nei moduli che seguono

* Nome descrittivo nel messaggio di posta elettronica "Microsoft per conto della demo CONTOSO"
* Riga dell'oggetto nel messaggio di posta elettronica "Codice di verifica della e-mail dell'account di demo CONTOSO"

## <a name="next-steps"></a>Passaggi successivi

I collegamenti seguenti forniscono altre informazioni sull'uso della reimpostazione delle password con Azure AD

* [**Guida introduttiva**](active-directory-passwords-getting-started.md) - iniziare a usare la gestione self-service delle password di Azure AD 
* [**Licenze**](active-directory-passwords-licensing.md) - configurare le licenze di Azure AD
* [**Dati** ](active-directory-passwords-data.md) - informazioni sui dati necessari e su come vengono usati per la gestione delle password
* [**Implementazione**](active-directory-passwords-best-practices.md) - pianificare e distribuire agli utenti la reimpostazione password self-service usando le istruzioni disponibili in questo articolo
* [**Criteri**](active-directory-passwords-policy.md) - comprendere e impostare i criteri password di Azure AD
* [**Writeback delle password**](active-directory-passwords-writeback.md): funzionamento del writeback delle password con la directory locale
* [**Creazione di report**](active-directory-passwords-reporting.md) - verificare se, quando e dove gli utenti accedono alla reimpostazione password self-service
* [**Approfondimento tecnico**](active-directory-passwords-how-it-works.md) - approfondimento sul funzionamento
* [**Domande frequenti**](active-directory-passwords-faq.md) - come? Perché? Cosa? Dove? Chi? Quando? - Risposte alle domande che si sarebbero sempre volute porre
* [**Risoluzione dei problemi**](active-directory-passwords-troubleshoot.md) - informazioni su come risolvere i problemi comuni con la reimpostazione password self-service


