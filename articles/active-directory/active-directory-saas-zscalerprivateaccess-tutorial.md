---
title: 'Esercitazione: Integrazione di Azure Active Directory con Zscaler Private Access (ZPA) | Microsoft Docs'
description: Informazioni su come configurare l&quot;accesso Single Sign-On tra Azure Active Directory e Zscaler Private Access (ZPA).
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 83711115-1c4f-4dd7-907b-3da24b37c89e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: c81f23603cfbbc916c148c61bd3de52ef1db36ac
ms.openlocfilehash: 5c598bfa5b6725d21a89df54dbcb3314cc631d80
ms.lasthandoff: 02/15/2017


---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-private-access-zpa"></a>Esercitazione: Integrazione di Azure Active Directory con Zscaler Private Access (ZPA)

Questa esercitazione descrive come integrare Zscaler Private Access (ZPA) con Azure Active Directory (Azure AD).

L'integrazione di Zscaler Private Access (ZPA) con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a Zscaler Private Access (ZPA)
- È possibile abilitare gli utenti per l'accesso automatico a Zscaler Private Access (ZPA) (Single Sign-On) con i propri account Azure AD
- È possibile gestire gli account da una posizione centrale: il portale di gestione di Azure

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con Zscaler Private Access (ZPA), sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD.
- Una sottoscrizione attiva con accesso Single Sign-on per Zscaler Private Access (ZPA)


> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.


A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione, a meno che non sia necessario.
- Se non è disponibile un ambiente di prova di Azure AD, è possibile ottenere una versione di prova di un mese [qui](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede i due blocchi predefiniti seguenti:

1. Aggiunta di Zscaler Private Access (ZPA) dalla raccolta
2. Configurazione e test dell'accesso Single Sign-On di Azure AD


## <a name="adding-zscaler-private-access-zpa-from-the-gallery"></a>Aggiunta di Zscaler Private Access (ZPA) dalla raccolta
Per configurare l'integrazione di Zscaler Private Access (ZPA) in Azure AD, è necessario aggiungere Zscaler Private Access (ZPA) dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere Zscaler Private Access (ZPA) dalla raccolta, seguire questa procedura:**

1. Nel **[portale di gestione di Azure](https://portal.azure.com)** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro. 

    ![Active Directory][1]

2. Passare ad **Applicazioni aziendali**. Andare quindi a **Tutte le applicazioni**.

    ![Applicazioni][2]
    
3. Fare clic sul pulsante **Aggiungi** nella parte superiore della finestra di dialogo.

    ![Applicazioni][3]

4. Nella casella di ricerca digitare **Zscaler Private Access (ZPA)**.

    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_001.png)

5. Nel pannello dei risultati selezionare **Zscaler Private Access (ZPA)** e quindi fare clic sul pulsante **Aggiungi** per aggiungere l'applicazione.

    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurazione e test dell'accesso Single Sign-On di Azure AD
In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con Zscaler Private Access (ZPA) in base a un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente di Zscaler Private Access (ZPA) che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in Zscaler Private Access (ZPA).

La relazione di collegamento viene stabilita assegnando il valore di **nome utente** in Azure AD come valore di **Nome utente** in Zscaler Private Access (ZPA).

Per configurare e testare l'accesso Single Sign-On di Azure AD con Zscaler Private Access (ZPA), è necessario completare i blocchi predefiniti seguenti:

1. **[Configurazione dell'accesso Single Sign-On di Azure AD](#configuring-azure-ad-single-sign-on)** : per abilitare gli utenti all'uso di questa funzionalità.
2. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)** : per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
3. **[Creazione di un utente test di Zscaler Private Access (ZPA)](#creating-a-zscaler-private-access-(zpa)-test-user)**: per avere una controparte di Britta Simon in Zscaler Private Access (ZPA) collegata alla relativa rappresentazione in Azure AD.
4. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)** : per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
5. **[Testing Single Sign-On](#testing-single-sign-on)** : per verificare se la configurazione funziona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configurazione dell'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di gestione di Azure e viene configurato l'accesso Single Sign-On nell'applicazione Zscaler Private Access (ZPA).

**Per configurare l'accesso Single Sign-On di Azure AD con Zscaler Private Access (ZPA), seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **Zscaler Private Access (ZPA)** del portale di gestione di Azure fare clic su **Single Sign-On**.

    ![Configura accesso Single Sign-On][4]

2. Nella finestra di dialogo **Single Sign-On** in **Modalità** selezionare **Accesso basato su SAML** per abilitare l'accesso Single Sign-On.
 
    ![Configura accesso Single Sign-On](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_300.png)
    
3. Nella sezione **Zscaler Private Access (ZPA) Domain and URLs** (URL e dominio di Zscaler Private Access (ZPA) seguire questa procedura:
    
    ![Configura accesso Single Sign-On](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_01.png)

    a. Nella casella di testo **URL di accesso** digitare l'URL usando il modello seguente: `https://samlsp.private.zscaler.com/auth/login?domain=<your-domain-name>`

    b. Nella casella di testo **Identificatore** digitare `https://samlsp.private.zscaler.com/auth/metadata`

    > [!NOTE] 
    > Si noti che questi non sono i valori reali. È necessario aggiornare questi valori con l'identificatore e l'URL di accesso effettivi. In questo caso è consigliabile usare in Identificatore il valore univoco dell'URL. Contattare il [team di supporto di Zscaler Private Access (ZPA)](https://help.zscaler.com/zpa-submit-ticket) per ottenere questi valori.

4. Nella sezione **Certificato di firma SAML** fare clic su **Crea nuovo certificato**.

    ![Configura accesso Single Sign-On](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_400.png)     

5. Nella finestra di dialogo **Crea nuovo certificato** fare clic sull'icona del calendario e selezionare una **data di scadenza**. Fare quindi clic sul pulsante **Salva**.

    ![Configura accesso Single Sign-On](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_500.png)

6. Nella sezione **Certificato di firma SAML** selezionare **Rendi attivo il certificato nuovo** e fare clic sul pulsante **Salva**.

    ![Configura accesso Single Sign-On](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_02.png)

7. Nella finestra popup **Certificato di rollover** fare clic su **OK**.

    ![Configura accesso Single Sign-On](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_600.png)

8. Nella sezione **Certificato di firma SAML** fare clic su **XML di metadati** e quindi salvare il file dei metadati nel computer.

    ![Configura accesso Single Sign-On](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_03.png) 

9. In un'altra finestra del Web browser accedere al sito aziendale di Zscaler Private Access (ZPA) come amministratore.

10. Passare ad **Amministratore** e quindi fare clic su **Idp Configuration** (Configurazione IdP).

    ![Configurazione accesso Single Sign-On sul lato app](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_04.png)

11. Nella sezione **Idp Configuration** (Configurazione IdP) fare clic su **Add New IDP Configuration** (Aggiungi nuova configurazione IdP).

    ![Configurazione accesso Single Sign-On sul lato app](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_05.png)

12. Nella sezione **New IDP Configuration** (Nuova configurazione IdP) eseguire la procedura seguente:

    ![Configurazione accesso Single Sign-On sul lato app](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_06.png)

    a. Per caricare il file di metadati scaricato, fare clic su **Seleziona file**.

    b. Fare clic sul pulsante **Salva** .
    


### <a name="creating-an-azure-ad-test-user"></a>Creazione di un utente test di Azure AD
Questa sezione descrive come creare un utente test chiamato Britta Simon nel portale di gestione di Azure.

![Creare un utente di Azure AD][100]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di gestione di Azure** fare clic sull'icona di **Azure Active Directory** nel riquadro di spostamento sinistro.

    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_01.png) 

2. Andare a **Utenti e gruppi** e fare clic su **Tutti gli utenti** per visualizzare l'elenco di utenti.
    
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_02.png) 

3. Nella parte superiore della finestra di dialogo fare clic su **Aggiungi** per aprire la finestra di dialogo **Utente**.
 
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_03.png) 

4. Nella pagina della finestra di dialogo **Utente** eseguire la procedura seguente:
 
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_04.png) 

    a. Nella casella di testo **Nome** digitare **BrittaSimon**.

    b. Nella casella di testo **Nome utente** digitare l'**indirizzo di posta elettronica** di BrittaSimon.

    c. Selezionare **Mostra password** e prendere nota del valore della **Password**.

    d. Fare clic su **Create**(Crea). 



### <a name="creating-a-zscaler-private-access-zpa-test-user"></a>Creazione di un utente di test Zscaler Private Access (ZPA)

In questa sezione viene creato un utente di nome Britta Simon in a Zscaler Private Access (ZPA). Consultare il [team di supporto di Zscaler Private Access (ZPA)](https://help.zscaler.com/zpa-submit-ticket) per aggiungere utenti sulla piattaforma a Zscaler Private Access (ZPA).


### <a name="assigning-the-azure-ad-test-user"></a>Assegnazione dell'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure, concedendole l'accesso a Zscaler Private Access (ZPA).

![Assegna utente][200] 

**Per assegnare Britta Simon a Zscaler Private Access (ZPA), seguire questa procedura:**

1. Nel portale di gestione di Azure aprire la visualizzazione applicazioni, passare alla visualizzazione directory e andare ad **Applicazioni aziendali**, quindi fare clic su **Tutte le applicazioni**.

    ![Assegna utente][201] 

2. Nell'elenco delle applicazioni, selezionare **Zscaler Private Access (ZPA)**.

    ![Configura accesso Single Sign-On](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_50.png) 

3. Scegliere **Utenti e gruppi** dal menu a sinistra.

    ![Assegna utente][202] 

4. Fare clic sul pulsante **Aggiungi**. Selezionare quindi **Utenti e gruppi** nella finestra di dialogo **Aggiungi assegnazione**.

    ![Assegna utente][203]

5. Nella finestra di dialogo **Utenti e gruppi** selezionare **Britta Simon** nell'elenco Utenti.

6. Fare clic sul pulsante **Seleziona** nella finestra di dialogo **Utenti e gruppi**.

7. Fare clic sul pulsante **Assegna** nella finestra di dialogo **Aggiungi assegnazione**.
    


### <a name="testing-single-sign-on"></a>Test dell'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro Zscaler Private Access (ZPA) nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione Zscaler Private Access (ZPA).


## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_203.png
