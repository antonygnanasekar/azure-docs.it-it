---
title: 'Esercitazione: Integrazione di Azure Active Directory con Mimecast Admin Console | Documentazione Microsoft'
description: Informazioni su come utilizzare Mimecast Admin Console con Azure Active Directory per abilitare l&quot;accesso Single Sign-On, il provisioning automatizzato e altro ancora.
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 81c50614-f49b-4bbc-97d5-3cf77154305f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/01/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 84f909bf26aca70b361a87c0d430b686a068054b
ms.openlocfilehash: 5a6cb773b557b525f8eb0c75a19feb70cab86c6b
ms.lasthandoff: 02/17/2017


---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a>Esercitazione: Integrazione di Azure Active Directory con Mimecast Admin Console
In questa esercitazione viene illustrata l'integrazione di Azure e Mimecast Admin Console.  
Per lo scenario descritto in questa esercitazione si presuppone che l'utente disponga di quanto segue:

* Sottoscrizione di Azure valida
* Sottoscrizione di Mimecast Admin Console abilitata per l'accesso Single Sign-On (SSO)

Al termine dell'esercitazione, gli utenti di Azure AD assegnati a Mimecast Admin Console saranno in grado di eseguire l’accesso Single Sign-On all'applicazione tramite il sito aziendale di Mimecast Admin Console (accesso avviato dal provider di servizi) o seguendo le istruzioni riportate in [Introduzione al Pannello di accesso](active-directory-saas-access-panel-introduction.md)

Lo scenario descritto in questa esercitazione include i blocchi predefiniti seguenti:

1. Abilitazione dell'integrazione dell'applicazione per Mimecast Admin Console
2. Configurazione dell'accesso Single Sign-On
3. Configurazione del provisioning utente
4. Assegnazione degli utenti

![Scenario](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795008.png "Scenario")

## <a name="enabling-the-application-integration-for-mimecast-admin-console"></a>Abilitazione dell'integrazione dell'applicazione per Mimecast Admin Console
Questa sezione descrive come abilitare l'integrazione dell'applicazione per Mimecast Admin Console.

**Ecco come abilitare l'integrazione dell'applicazione per Mimecast Admin Console:**

1. Nel portale di Azure classico fare clic su **Active Directory**nel riquadro di spostamento sinistro.
   
   ![Active Directory](./media/active-directory-saas-mimecast-admin-console-tutorial/IC700993.png "Active Directory")
2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.
3. Per aprire la visualizzazione applicazioni, nella visualizzazione directory fare clic su **Applications** nel menu superiore.
   
   ![Applicazioni](./media/active-directory-saas-mimecast-admin-console-tutorial/IC700994.png "Applicazioni")
4. Fare clic su **Add** nella parte inferiore della pagina.
   
   ![Aggiungere un'applicazione](./media/active-directory-saas-mimecast-admin-console-tutorial/IC749321.png "Aggiungere un'applicazione")
5. Nella finestra di dialogo **Come procedere** fare clic su **Aggiungere un'applicazione dalla raccolta**.
   
   ![Aggiungere un'applicazione dalla raccolta](./media/active-directory-saas-mimecast-admin-console-tutorial/IC749322.png "Aggiungere un'applicazione dalla raccolta")
6. Nella **casella di ricerca** digitare **Mimecast Admin Console**.
   
   ![Raccolta di applicazioni](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795009.png "Raccolta di applicazioni")
7. Nel riquadro dei risultati selezionare **Mimecast Admin Console**, quindi fare clic su **Completa** per aggiungere l'applicazione.
   
   ![Console di amministrazione di Mimecast](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795010.png "Console di amministrazione di Mimecast")
   
## <a name="configuring-single-sign-on"></a>Configurazione dell'accesso Single Sign-On

Questa sezione descrive come consentire agli utenti di eseguire l'autenticazione a Mimecast Admin Console con il proprio account in Azure AD utilizzando la federazione basata sul protocollo SAML.  

Come parte di questa procedura, verrà richiesto di creare un file di certificato con codifica Base&64;.  
Se non si ha familiarità con questa procedura, vedere il video che illustra [come convertire un certificato binario in un file di testo](http://youtu.be/PlgrzUZ-Y1o).

**Per configurare l'accesso Single Sign-On, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **Mimecast Admin Console** nel portale di Azure classico fare clic su **Configura accesso Single Sign-On** per aprire la finestra di dialogo **Configura accesso Single Sign-On**.
   
   ![Configurare l'accesso Single Sign-On](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795011.png "Configurare l'accesso Single Sign-On")
2. Nella pagina **Stabilire come si desidera che gli utenti accedano a Mimecast Admin Console** selezionare **Single Sign-On di Microsoft Azure AD**, quindi fare clic su **Avanti**.
   
   ![Configurare l'accesso Single Sign-On](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795012.png "Configurare l'accesso Single Sign-On")
3. Nella pagina **Configura URL app**, nella casella di testo **URL di accesso a Mimecast Admin Console**, digitare l'URL utilizzato dagli utenti per accedere all'applicazione Mimecast Admin Console (ad esempio, "https://webmail-uk.mimecast.com" o "https://webmail-us.mimecast.com"), quindi fare clic su **Avanti**.
   
   >[!NOTE]
   >L’URL di accesso è specifico dell’area geografica. 
   > 
   
   ![Configurare l'URL dell'app](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795013.png "Configurare l'URL dell'app")
4. Nella pagina **Configura accesso Single Sign-On in Mimecast Admin Console** per scaricare il certificato fare clic su **Download certificato**, quindi salvare il file di certificato localmente nel computer.
   
   ![Configurare l'accesso Single Sign-On](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795014.png "Configurare l'accesso Single Sign-On")
5. In un'altra finestra del Web browser accedere a Mimecast Admin Console come amministratore.
6. Passare a **Services \> Application**.
   
   ![Servizi](./media/active-directory-saas-mimecast-admin-console-tutorial/IC794998.png "Servizi")
7. Fare clic su **Authentication Profiles**.
   
   ![Profili di autenticazione](./media/active-directory-saas-mimecast-admin-console-tutorial/IC794999.png "Profili di autenticazione")
8. Fare clic su **New Authentication Profile**.
   
   ![Nuovi profili di autenticazione](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795000.png "Nuovi profili di autenticazione")
9. Nella sezione **Authentication Profile** , eseguire la procedura seguente:
   
   ![Profilo di autenticazione](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795015.png "Profilo di autenticazione")
   
   1. Nella casella di testo **Description** digitare un nome per la configurazione.
   2. Selezionare **Enforce SAML Authentication for Mimecast Admin Console**.
   3. Come **Provider** selezionare **Azure Active Directory**.
   4. Nella pagina **Configura accesso Single Sign-On in Mimecast Admin Console** del portale di Azure classico copiare il valore **URL autorità di certificazione** e incollarlo nella casella di testo **Issuer URL** (URL autorità di certificazione).
   5. Nella pagina **Configura accesso Single Sign-On in Mimecast Admin Console** del portale di Azure classico copiare il valore **URL accesso remoto** e incollarlo nella casella di testo **Login URL** (URL di accesso).
   6. Nella pagina **Configura accesso Single Sign-On in Mimecast Admin Console** del portale di Azure classico copiare il valore **URL accesso remoto** e incollarlo nella casella di testo **Logout URL** (URL di accesso).  
      >[!NOTE]
      >Il valore di Login URL e il valore Logout URL per Mimecast Admin Console sono identici. 
      > 
   7. Creare un file **con codifica Base&64;** dal certificato scaricato.  
      
      >[!TIP]
      >Per informazioni dettagliate, vedere il video che illustra [come convertire un certificato binario in un file di testo](http://youtu.be/PlgrzUZ-Y1o). 
      > 
   8. Aprire il certificato con codifica Base&64; nel Blocco note, rimuovere la prima riga ("*--*") e l'ultima riga ("*--*"), copiare il contenuto rimanente negli Appunti e incollarlo nella casella di testo **Identity Provider Certificate (Metadata)** (Certificato del provider di identità (Metadati)).
   9. Selezionare **Allow Single Sign On**.
   10. Fare clic su **Salva**.
10. Nel portale di Azure classico selezionare la conferma della configurazione dell'accesso Single Sign-On e quindi fare clic su **Complete** per chiudere la finestra di dialogo **Configura accesso Single Sign-On**.
    
    ![Configurare l'accesso Single Sign-On](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795016.png "Configurare l'accesso Single Sign-On")
    
## <a name="configuring-user-provisioning"></a>Configurazione del provisioning utente

Per consentire agli utenti di Azure AD di accedere a Mimecast Admin Console, è necessario eseguirne il provisioning in Mimecast Admin Console. Nel caso di Mimecast Admin Console, il provisioning è un’attività manuale.

* Per poter creare gli utenti è necessario registrare un dominio.

**Per configurare il provisioning utenti, seguire questa procedura:**

1. Accedere a **Mimecast Admin Console** come amministratore.
2. Accedere a **Directories \> Internal**.
   
   ![Directory](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795003.png "Directory")
3. Fare clic su **Register New Domain**.
   
   ![Registra nuovo dominio](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795004.png "Registra nuovo dominio")
4. Dopo aver creato il nuovo dominio, fare clic su **New Address**.
   
   ![Nuovo indirizzo](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795005.png "nuovo indirizzo")
5. Nella finestra di dialogo del nuovo indirizzo, seguire questa procedura:
   
   ![Salvare](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795006.png "Salvare")
   
   1. Nelle caselle di testo corrispondenti digitare gli attributi **indirizzo di posta elettronica**, **nome globale**, **password** e **conferma password** di un account ADD valido di cui si intende eseguire il provisioning.
   2. Fare clic su **Save**.

>[!NOTE]
>È possibile utilizzare qualsiasi altro strumento di creazione di account utente di Mimecast Admin Console o le API fornite da Mimecast Admin Console per eseguire il provisioning degli account utente di AAD. 
> 

## <a name="assigning-users"></a>Assegnazione degli utenti
Per testare la configurazione, è necessario concedere l'accesso all'applicazione agli utenti di Azure AD a cui si vuole consentirne l'uso, assegnando tali utenti all'applicazione.

**Per assegnare gli utenti a Mimecast Admin Console, seguire questa procedura:**

1. Nel portale di Azure classico creare un account di test.
2. Nella pagina di integrazione dell'applicazione **Mimecast Admin Console** fare clic su **Assegna utenti**.
   
   ![Assegnare utenti](./media/active-directory-saas-mimecast-admin-console-tutorial/IC795017.png "Assegnare utenti")
3. Selezionare l'utente di test, fare clic su **Assegna** e quindi su **Sì** per confermare l'assegnazione.
   
   ![Sì](./media/active-directory-saas-mimecast-admin-console-tutorial/IC767830.png "Sì")

Per testare le impostazioni di Single Sign-On, aprire il pannello di accesso. Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](active-directory-saas-access-panel-introduction.md).


