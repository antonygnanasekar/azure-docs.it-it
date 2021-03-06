---
title: 'Esercitazione: Integrazione di Azure Active Directory con Zendesk | Documentazione Microsoft'
description: Informazioni su come usare Zendesk con Azure Active Directory per abilitare l&quot;accesso Single Sign-On, il provisioning automatizzato e altro ancora.
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 9d7c91e5-78f5-4016-862f-0f3242b00680
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 1c22e4fc17226578aaaf272fdf79178da65c63c2
ms.openlocfilehash: 4ef08fb592ff8558fa779d628945d14144dc09b7
ms.lasthandoff: 02/23/2017


---
# <a name="tutorial-azure-active-directory-integration-with-zendesk"></a>Esercitazione: Integrazione di Azure Active Directory con Zendesk
Questa esercitazione descrive l'integrazione di Azure e Zendesk.  
Per lo scenario descritto in questa esercitazione si presuppone che l'utente disponga di quanto segue:

* Sottoscrizione di Azure valida
* Tenant di Zendesk

Al termine dell'esercitazione, gli utenti di Azure AD assegnati a Zendesk potranno accedere all'applicazione tramite il sito aziendale di Zendesk (accesso avviato dal provider di servizi) o seguendo le istruzioni riportate in [Introduzione al Pannello di accesso](active-directory-saas-access-panel-introduction.md).

Lo scenario descritto in questa esercitazione include i blocchi predefiniti seguenti:

1. Abilitazione dell'integrazione dell'applicazione Zendesk
2. Configurazione dell'accesso Single Sign-On
3. Configurazione del provisioning utente
4. Assegnazione degli utenti

![Scenario](./media/active-directory-saas-zendesk-tutorial/IC773083.png "Scenario")

## <a name="enabling-the-application-integration-for-zendesk"></a>Abilitazione dell'integrazione dell'applicazione Zendesk
Questa sezione descrive come abilitare l'integrazione dell'applicazione per Zendesk.

### <a name="to-enable-the-application-integration-for-zendesk-perform-the-following-steps"></a>Per abilitare l'integrazione dell'applicazione per Zendesk, seguire questa procedura:
1. Nel portale di gestione di Azure fare clic su **Active Directory**nel pannello di navigazione sinistro.
   
    ![Active Directory](./media/active-directory-saas-zendesk-tutorial/IC700993.png "Active Directory")

2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.

3. Per aprire la visualizzazione applicazioni, nella visualizzazione directory fare clic su **Applications** nel menu superiore.
   
    ![Applicazioni](./media/active-directory-saas-zendesk-tutorial/IC700994.png "Applicazioni")

4. Fare clic su **Add** nella parte inferiore della pagina.
   
    ![Aggiungere un'applicazione](./media/active-directory-saas-zendesk-tutorial/IC749321.png "Aggiungere un'applicazione")

5. Nella finestra di dialogo **Come procedere** fare clic su **Aggiungere un'applicazione dalla raccolta**.
   
    ![Aggiungere un'applicazione dalla raccolta](./media/active-directory-saas-zendesk-tutorial/IC749322.png "Aggiungere un'applicazione dalla raccolta")

6. Nella **casella di ricerca** digitare **Zendesk**.
   
    ![Raccolta di applicazioni](./media/active-directory-saas-zendesk-tutorial/IC773084.png "Raccolta di applicazioni")

7. Nel riquadro dei risultati selezionare **Zendesk** e quindi fare clic su **Completa** per aggiungere l'applicazione.
   
    ![Zendesk](./media/active-directory-saas-zendesk-tutorial/IC773085.png "Zendesk")

## <a name="configuring-single-sign-on"></a>Configurazione dell'accesso Single Sign-On
Questa sezione descrive come consentire agli utenti di eseguire l'autenticazione a Zendesk tramite il proprio account in Azure AD usando la federazione basata sul protocollo SAML.  
La configurazione dell'accesso Single Sign-On per Zendesk richiede di recuperare un valore di identificazione personale da un certificato.  
Se non si ha familiarità con questa procedura, vedere [Procedura: recuperare l'identificazione personale di un certificato](http://youtu.be/YKQF266SAxI).

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Per configurare l'accesso Single Sign-On, seguire questa procedura:
1. Nella pagina di integrazione dell'applicazione **Zendesk** del portale di Azure AD fare clic su **Configura accesso Single Sign-On** per aprire la finestra di dialogo **Configura accesso Single Sign-On**.
   
    ![Single Sign-On](./media/active-directory-saas-zendesk-tutorial/IC773086.png "Single Sign-On")

2. Nella pagina **Stabilire come si desidera che gli utenti accedano a Zendesk** selezionare **Single Sign-On di Microsoft Azure AD** e quindi fare clic su **Avanti**.
   
    ![Configurare l'accesso Single Sign-On](./media/active-directory-saas-zendesk-tutorial/IC773087.png "Configurare l'accesso Single Sign-On")

3. Nella pagina **Configura URL app** seguire questa procedura:
   
    ![Configurare l'URL dell'app](./media/active-directory-saas-zendesk-tutorial/IC773088.png "Configurare l'URL dell'app")
   
    a. Nella casella di testo **URL di accesso Zendesk** digitare l'URL usando il modello seguente: `https://<tenant-name>.zendesk.com`
   
    b. Fare clic su **Avanti**.

4. Nella pagina **Configura accesso Single Sign-On in Zendesk** fare clic su **Download certificato** per scaricare il file del certificato e quindi salvarlo nel computer locale.
   
    ![Configurare l'accesso Single Sign-On](./media/active-directory-saas-zendesk-tutorial/IC777534.png "Configurare l'accesso Single Sign-On")

5. In un'altra finestra del Web browser accedere al sito aziendale di Zendesk come amministratore.

6. Fare clic su **Admin**.

7. Nel riquadro di spostamento sinistro fare clic su **Impostazioni** e quindi su **Sicurezza**.
   
    ![Sicurezza](./media/active-directory-saas-zendesk-tutorial/IC773089.png "Sicurezza")

8. Nella pagina **Sicurezza** fare clic sulla scheda **Amministratore e Agenti**.

9. Selezionare **Single sign-on (SSO) e SAML** e quindi **SAML**.

10. Nella finestra di dialogo **Configura accesso Single Sign-On in Zendesk** del portale di Azure AD copiare il valore di **URL SSO SAML** e quindi incollarlo nella casella di testo **SAML SSO URL**.

11. Nella finestra di dialogo **Configura accesso Single Sign-On in Zendesk** del portale di Azure AD copiare il valore di **URL di disconnessione remota** e quindi incollarlo nella casella di testo **Remote Logout URL**.
    
    ![Single Sign-On](./media/active-directory-saas-zendesk-tutorial/IC773090.png "Single Sign-On")

12. Copiare il valore **Identificazione personale** dal certificato esportato e quindi incollarlo nella casella di testo **Impronta digitale del certificato**.
    
    > [!TIP]
    > Per informazioni dettagliate, vedere [come recuperare un valore di identificazione personale del certificato](http://youtu.be/YKQF266SAxI)
    > 
    > 

13. Fare clic su **Save**.

14. Nel portale di Azure AD selezionare la conferma della configurazione dell'accesso Single Sign-On, quindi fare clic su **Completa** per chiudere la finestra di dialogo **Configura accesso Single Sign-On**.
    
    ![Configurare l'accesso Single Sign-On](./media/active-directory-saas-zendesk-tutorial/IC773093.png "Configurare l'accesso Single Sign-On")

## <a name="configuring-user-provisioning"></a>Configurazione del provisioning utente
Per consentire agli utenti di Azure AD di accedere a **Zendesk**, è necessario eseguirne il provisioning in **Zendesk**.  
Nel caso di **Zendesk**, il provisioning è un'attività manuale.

### <a name="to-provision-a-user-account-to-zendesk-perform-the-following-steps"></a>Per eseguire il provisioning di un account utente in Zendesk, seguire questa procedura:
1. Accedere al tenant di **Zendesk** .

2. Selezionare la scheda **Customer List** .

3. Selezionare la scheda **User** e fare clic su **Add**.
   
    ![Aggiungere un utente](./media/active-directory-saas-zendesk-tutorial/IC773632.png "Aggiungere un utente")
4. Digitare l'indirizzo di posta elettronica di un account Azure AD esistente di cui si vuole eseguire il provisioning e quindi fare clic su **Save**.
   
    ![Nuovo utente](./media/active-directory-saas-zendesk-tutorial/IC773633.png "Nuovo utente")

> [!NOTE]
> È possibile usare qualsiasi altro strumento o API di creazione di account utente fornita da Zendesk per eseguire il provisioning degli account utente Azure AD.
> 
> 

## <a name="assigning-users"></a>Assegnazione degli utenti
Per testare la configurazione, è necessario concedere l'accesso all'applicazione agli utenti di Azure AD a cui si vuole consentirne l'uso, assegnando tali utenti all'applicazione.

### <a name="to-assign-users-to-zendesk-perform-the-following-steps"></a>Per assegnare gli utenti a Zendesk, seguire questa procedura:
1. Nel portale di Azure AD creare un account di test.

2. Nella pagina di integrazione dell'applicazione **Zendesk** fare clic su **Assegna utenti**.
   
    ![Assegnare utenti](./media/active-directory-saas-zendesk-tutorial/IC773094.png "Assegnare utenti")

3. Selezionare l'utente di test, fare clic su **Assegna** e quindi su **Sì** per confermare l'assegnazione.
   
    ![Sì](./media/active-directory-saas-zendesk-tutorial/IC767830.png "Sì")

Per testare le impostazioni di Single Sign-On, aprire il pannello di accesso. Per altre informazioni sul pannello di accesso, vedere [Introduzione al Pannello di accesso](active-directory-saas-access-panel-introduction.md).


