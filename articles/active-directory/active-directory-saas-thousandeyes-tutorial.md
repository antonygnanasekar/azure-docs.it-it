---
title: 'Esercitazione: Integrazione di Azure Active Directory con ThousandEyes | Documentazione Microsoft'
description: Informazioni su come usare ThousandEyes con Azure Active Directory per abilitare l&quot;accesso Single Sign-On, il provisioning automatizzato e altro ancora.
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 790e3f1e-1591-4dd6-87df-590b7bf8b4ba
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 3/09/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: 7bc96e6a711c70f9c5fa5daa4e059d9d7c04a134
ms.lasthandoff: 03/28/2017


---
# <a name="tutorial-azure-active-directory-integration-with-thousandeyes"></a>Esercitazione: Integrazione di Azure Active Directory con ThousandEyes
L'obiettivo di questa esercitazione è di illustrare come impostare il single sign-on tra Azure Active Directory (Azure AD) e ThousandEyes.

Per lo scenario descritto in questa esercitazione si presuppone che l'utente disponga di quanto segue:

* Sottoscrizione di Azure valida
* Sottoscrizione di ThousandEyes abilitata per l'accesso Single Sign-On (SSO)

Al termine dell'esercitazione, gli utenti di AAD assegnati a ThousandEyes saranno in grado di eseguire l'accesso Single Sign-On all'applicazione tramite il sito aziendale di ThousandEyes (accesso avviato dal provider di servizi) o tramite il pannello di accesso ad AAD.

1. Abilitazione dell'integrazione dell'applicazione per ThousandEyes
2. Configurazione dell'accesso Single Sign-On
3. Configurazione del provisioning utente
4. Assegnazione degli utenti

![Scenario](./media/active-directory-saas-thousandeyes-tutorial/IC790059.png "Scenario")

## <a name="enable-the-application-integration-for-thousandeyes"></a>Abilitare l'integrazione dell'applicazione per ThousandEyes
In questa sezione viene descritto come abilitare l'integrazione dell'applicazione per ThousandEyes.

**Per abilitare l'integrazione dell'applicazione per ThousandEyes, eseguire la procedura seguente:**

1. Nel portale di Azure classico fare clic su **Active Directory**nel riquadro di spostamento sinistro.
   
    ![Active Directory](./media/active-directory-saas-thousandeyes-tutorial/IC700993.png "Active Directory")

2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.

3. Per aprire la visualizzazione applicazioni, nella visualizzazione directory fare clic su **Applications** nel menu superiore.
   
    ![Applicazioni](./media/active-directory-saas-thousandeyes-tutorial/IC700994.png "Applicazioni")

4. Fare clic su **Add** nella parte inferiore della pagina.
   
    ![Aggiungere un'applicazione](./media/active-directory-saas-thousandeyes-tutorial/IC749321.png "Aggiungere un'applicazione")

5. Nella finestra di dialogo **Come procedere** fare clic su **Aggiungere un'applicazione dalla raccolta**.
   
    ![Aggiungere un'applicazione dalla raccolta](./media/active-directory-saas-thousandeyes-tutorial/IC749322.png "Aggiungere un'applicazione dalla raccolta")

6. Nella **casella di ricerca** digitare **ThousandEyes**.
   
    ![Raccolta di applicazioni](./media/active-directory-saas-thousandeyes-tutorial/IC790060.png "Raccolta di applicazioni")
7. Nel riquadro dei risultati selezionare **ThousandEyes**, quindi fare clic su **Completa** per aggiungere l'applicazione.
   
    ![ThousandEyes](./media/active-directory-saas-thousandeyes-tutorial/IC790061.png "ThousandEyes")

## <a name="configure-single-sign-on"></a>Configura accesso Single Sign-On
In questa sezione viene descritto come consentire agli utenti di eseguire l'autenticazione a ThousandEyes tramite il relativo account in Azure Active Directory utilizzando la federazione basata sul protocollo SAML.

**Per configurare l'accesso Single Sign-On, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **ThousandEyes** del portale di Azure classico fare clic su **Configura accesso Single Sign-On** per aprire la finestra di dialogo **Configura Single Sign-On**.
   
    ![Configurare l'accesso Single Sign-On](./media/active-directory-saas-thousandeyes-tutorial/IC790062.png "Configurare l'accesso Single Sign-On")

2. Nella pagina **Stabilire come si desidera che gli utenti accedano a ThousandEyes** selezionare **Single Sign-On di Microsoft Azure AD**, quindi fare clic su **Avanti**.
   
    ![Configurare l'accesso Single Sign-On](./media/active-directory-saas-thousandeyes-tutorial/IC790063.png "Configurare l'accesso Single Sign-On")

3. Nella casella di testo **URL di accesso a ThousandEyes** della pagina **Configura URL app** digitare l'URL che gli utenti cercano di usare per accedere all'applicazione ThousandEyes, ad esempio "*https://app.thousandeyes.com/login/sso*", e quindi fare clic su **Avanti**. 
   
    ![Configurare l'URL dell'app](./media/active-directory-saas-thousandeyes-tutorial/IC790064.png "Configurare l'URL dell'app")

4. Nella pagina **Configura accesso Single Sign-On in ThousandEyes** fare clic su **Scarica certificato** per scaricare il certificato e quindi salvare il file di certificato localmente nel computer.
   
    ![Configurare l'accesso Single Sign-On](./media/active-directory-saas-thousandeyes-tutorial/IC790065.png "Configurare l'accesso Single Sign-On")

5. In un'altra finestra del browser Web accedere al sito aziendale di **ThousandEyes** come amministratore.

6. Nel menu in alto fare clic su **Impostazioni**.
   
    ![Impostazioni](./media/active-directory-saas-thousandeyes-tutorial/IC790066.png "Impostazioni")

7. Fare clic su **Account**
   
    ![Account](./media/active-directory-saas-thousandeyes-tutorial/IC790067.png "Account")

8. Fare clic sulla scheda **Security & Authentication** (Sicurezza e autenticazione).
   
    ![Sicurezza e autenticazione](./media/active-directory-saas-thousandeyes-tutorial/IC790068.png "Security e autenticazione")

9. Nella sezione **Setup Single Sign-On** eseguire la procedura seguente:
   
    ![Configurare l'accesso Single Sign-On](./media/active-directory-saas-thousandeyes-tutorial/IC790069.png "Configurare l'accesso Single Sign-On")
  1. Selezionare **Enable Single Sign-On**.
  2. Nella pagina **Configura accesso Single Sign-On in ThousandEyes** del portale di Microsoft Azure classico copiare il valore di **URL accesso remoto** e incollarlo nella casella di testo **Login Page URL** (URL pagina di accesso).
  3. Nella pagina **Configura accesso Single Sign-On in ThousandEyes** del portale di Microsoft Azure classico copiare il valore di **URL disconnessione remota** e incollarlo nella casella di testo **Logout Page URL** (URL pagina di disconnessione).
  4. Nella pagina **Configura accesso Single Sign-On in ThousandEyes** del portale di Microsoft Azure classico copiare il valore di **URL autorità di certificazione** e incollarlo nella casella di testo **Identity Provider Issuer** (Autorità di certificazione provider di identità).
  5. In **Identity Provider Certificate** (Certificato provider di identità) fare clic su **Choose file** (Scegli file) e quindi caricare il certificato scaricato dal portale di Microsoft Azure classico.
  6. Fare clic su **Salva**.

10. Nel portale di Azure classico selezionare la conferma della configurazione dell'accesso Single Sign-On e quindi fare clic su **Complete** per chiudere la finestra di dialogo **Configura accesso Single Sign-On**.
    
    ![Configurare l'accesso Single Sign-On](./media/active-directory-saas-thousandeyes-tutorial/IC790070.png "Configurare l'accesso Single Sign-On")

## <a name="configure-user-provisioning"></a>Configura provisioning utenti
Per consentire agli utenti di Azure AD di accedere a ThousandEyes, è necessario eseguirne il provisioning in ThousandEyes.  
Nel caso di ThousandEyes, il provisioning è un'attività manuale.

**Per eseguire il provisioning di un account utente a ThousandEyes, eseguire la procedura seguente:**

1. Accedere al sito aziendale di ThousandEyes come amministratore.

2. Fare clic su **Impostazioni**.
   
    ![Impostazioni](./media/active-directory-saas-thousandeyes-tutorial/IC790066.png "Impostazioni")

3. Fare clic su **Account**.
   
    ![Account](./media/active-directory-saas-thousandeyes-tutorial/IC790067.png "Account")

4. Fare clic sulla scheda **Accounts & Users** (Account e utenti).
   
    ![Account e utenti](./media/active-directory-saas-thousandeyes-tutorial/IC790073.png "Account e utenti")

5. Nella sezione **Add Users & Accounts** (Account e utenti) eseguire la procedura seguente:
   
    ![Aggiungere account utente](./media/active-directory-saas-thousandeyes-tutorial/IC790074.png "Aggiungere account utente")   
  1. Digitare il **nome**, l'**indirizzo di posta elettronica** e altri dettagli di un account Azure Active Directory valido di cui si desidera eseguire il provisioning nelle caselle di testo correlate.
  2. Fare clic su **Add New User to Account**.
      
     >[!NOTE]
     >Il titolare dell’account AAD riceverà un messaggio di posta elettronica con un collegamento per confermare e attivare l'account.
     >  

>[!NOTE]
>È possibile usare qualsiasi altro strumento o API di creazione di account utente fornita da ThousandEyes per eseguire il provisioning degli account utente di AAD.
>  

## <a name="assign-users"></a>Assegna utenti
Per testare la configurazione, è necessario concedere l'accesso all'applicazione agli utenti di Azure AD a cui si vuole consentirne l'uso, assegnando tali utenti all'applicazione.

**Per assegnare gli utenti a ThousandEyes, eseguire la procedura seguente:**

1. Nel portale di Azure classico creare un account di test.

2. Nella pagina dell'integrazione dell'applicazione **ThousandEyes** fare clilc su **Assegna utenti**.
   
    ![Assegnare utenti](./media/active-directory-saas-thousandeyes-tutorial/IC790075.png "Assegnare utenti")

3. Selezionare l'utente di test, fare clic su **Assegna** e quindi su **Sì** per confermare l'assegnazione.
   
    ![Sì](./media/active-directory-saas-thousandeyes-tutorial/IC767830.png "Sì")

Per testare le impostazioni di Single Sign-On, aprire il pannello di accesso. Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](active-directory-saas-access-panel-introduction.md).


