---
title: 'Esercitazione: Integrazione di Azure Active Directory con JobScore | Documentazione Microsoft'
description: Informazioni su come usare JobScore con Azure Active Directory per abilitare l&quot;accesso Single Sign-On, il provisioning automatizzato e altro ancora.
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 30f51b32-e55c-4c66-96e8-50a2f9c2194a
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 7fcf4a3293da07b406216e2382e1a6d29f934f23
ms.openlocfilehash: 18ff96c76a4e2233edf734f84cce8adbf7716c8b


---

# <a name="tutorial-azure-active-directory-integration-with-jobscore"></a>Esercitazione: Integrazione di Azure Active Directory con JobScore

Questa esercitazione descrive come integrare JobScore con Azure Active Directory (Azure AD).

L'integrazione di JobScore con Azure AD offre i vantaggi seguenti:

- È possibile controllare in Azure AD chi può accedere a JobScore
- È possibile abilitare gli utenti per l'accesso automatico a JobScore (Single Sign-On) con i propri account Azure AD
- È possibile gestire gli account da una posizione centrale: il portale di Azure classico

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md).


## <a name="prerequisites"></a>Prerequisiti

Per configurare l'integrazione di Azure AD con JobScore, sono necessari gli elementi seguenti:

- Sottoscrizione di Azure AD.
- Sottoscrizione di JobScore abilitata per l'accesso Single Sign-On.


> [!NOTE]
> Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione.


A questo scopo, è consigliabile seguire le indicazioni seguenti:

- Non usare l'ambiente di produzione, a meno che non sia necessario.
- Se non si dispone di un ambiente di prova di Azure AD, è possibile ottenere una versione di valutazione di un mese [qui](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Descrizione dello scenario
In questa esercitazione viene eseguito il test dell'accesso Single Sign-On di Azure AD in un ambiente di test. Lo scenario descritto in questa esercitazione prevede i due blocchi predefiniti seguenti:

1. Aggiunta di JobScore dalla raccolta
2. Configurazione e test dell'accesso Single Sign-On di Azure AD


## <a name="adding-jobscore-from-the-gallery"></a>Aggiunta di JobScore dalla raccolta
Per configurare l'integrazione di JobScore in Azure AD, è necessario aggiungere JobScore dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere JobScore dalla raccolta, seguire questa procedura:**

1. Nel **portale di Azure classico**fare clic su **Active Directory**nel riquadro di spostamento sinistro. 

    ![Active Directory][1]

2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.

3. Per aprire la visualizzazione applicazioni, nella visualizzazione directory fare clic su **Applications** nel menu superiore.

    ![Applications][2]

4. Fare clic su **Add** nella parte inferiore della pagina.

    ![Applicazioni][3]

5. Nella finestra di dialogo **Come procedere** fare clic su **Aggiungere un'applicazione dalla raccolta**.

    ![Applicazioni][4]

6. Nella casella di ricerca digitare **JobScore**.

    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-jobscore-tutorial/tutorial_jobscore_001.png)

7. Nel riquadro dei risultati selezionare **JobScore** e quindi fare clic su **Completa** per aggiungere l'applicazione.

    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-jobscore-tutorial/tutorial_jobscore_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurazione e test dell'accesso Single Sign-On di Azure AD
In questa sezione viene configurato e testato l'accesso Single Sign-On di Azure AD con JobScore in base a un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso Single Sign-On, Azure AD deve conoscere qual è l'utente di JobScore che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in JobScore.

La relazione di collegamento viene stabilita assegnando il valore di **nome utente** in Azure AD come valore di **Username** (Nome utente) in JobScore.

Per configurare e testare l'accesso Single Sign-On di Azure AD con JobScore, è necessario eseguire le operazioni fondamentali seguenti:

1. **[Configurazione dell'accesso Single Sign-On di Azure AD](#configuring-azure-ad-single-sign-on)** : per abilitare gli utenti all'uso di questa funzionalità.
2. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)** : per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
3. **[Creazione di un utente test JobScore](#creating-a-jobscore-test-user)**: per avere una controparte di Britta Simon in JobScore collegata alla relativa rappresentazione in Azure AD.
4. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)** : per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
5. **[Testing Single Sign-On](#testing-single-sign-on)** : per verificare se la configurazione funziona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configurazione dell'accesso Single Sign-On di Azure AD

In questa sezione viene abilitato l'accesso Single Sign-On di Azure AD nel portale di Azure classico e viene configurato l'accesso Single Sign-On nell'applicazione JobScore.


**Per configurare l'accesso Single Sign-On di Azure AD con JobScore, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **JobScore** del portale di Azure classico fare clic su **Configura accesso Single Sign-On** per aprire la finestra di dialogo **Configura accesso Single Sign-On**.

    ![Configura accesso Single Sign-On][6]

2. Nella pagina **Stabilire come si desidera che gli utenti accedano a JobScore** selezionare **Single Sign-On di Azure AD** e quindi fare clic su **Avanti**.
 
    ![Configura accesso Single Sign-On](./media/active-directory-saas-jobscore-tutorial/tutorial_jobscore_02.png)

3. Nella pagina **Configurare le impostazioni dell'app** seguire questa procedura:

    ![Configura accesso Single Sign-On](./media/active-directory-saas-jobscore-tutorial/tutorial_jobscore_03.png)

    a. Nella casella di testo **URL di accesso** digitare l'URL usando il modello seguente: `https://hire.jobscore.com/auth/adfs/<company name>`

    b. Fare clic su **Next**.

    > [!NOTE] 
    > Si noti che questo non è il valore reale. È necessario aggiornare questo valore con l'URL di accesso effettivo. Per ottenere tale valore, contattare il [team di supporto di JobScore](emaiLto:support@jobscore.com).

4. Nella pagina **Configurare l'accesso Single Sign-On in JobScore** fare clic su **Scarica metadati** e quindi salvare il file nel computer:

    ![Configura accesso Single Sign-On](./media/active-directory-saas-jobscore-tutorial/tutorial_jobscore_04.png) 

5. Per ottenere la configurazione dell'accesso Single Sign-On per l'applicazione, contattare il [team di supporto di JobScore](emaiLto:support@jobscore.com) e fornire i **metadati** scaricati.

6. Nel portale classico selezionare la conferma della configurazione dell'accesso Single Sign-On e quindi fare clic su **Avanti**.

    ![Single Sign-On di Microsoft Azure AD][10]

7. Nella pagina **Conferma Single Sign-on** fare clic su **Completa**.  
  
    ![Single Sign-On di Microsoft Azure AD][11]


### <a name="creating-an-azure-ad-test-user"></a>Creazione di un utente test di Azure AD
Questa sezione descrive come creare un utente di test chiamato Britta Simon nel portale classico.

![Creare un utente di Azure AD][20]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure classico** fare clic su **Active Directory** nel riquadro di spostamento sinistro.

    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-jobscore-tutorial/create_aaduser_09.png) 

2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.

3. Per visualizzare l'elenco di utenti, fare clic su **Utenti**nel menu in alto.

    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-jobscore-tutorial/create_aaduser_03.png) 

4. Per aprire la finestra di dialogo **Aggiungi utente**, fare clic su **Aggiungi utente** nella barra degli strumenti in basso.
 
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-jobscore-tutorial/create_aaduser_04.png) 

5. Nella pagina **Informazioni sull'utente** seguire questa procedura:
 
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-jobscore-tutorial/create_aaduser_05.png) 

    a. In Tipo di utente selezionare Nuovo utente nell'organizzazione.

    b. Nella casella di testo **Nome utente** digitare **BrittaSimon**.

    c. Fare clic su **Avanti**.

6.  Nella pagina **Profilo utente** seguire questa procedura:

    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-jobscore-tutorial/create_aaduser_06.png) 

    a. Nella casella di testo **Nome** digitare **Britta**.  

    b. Nella casella di testo **Cognome** digitare **Simon**.

    c. Nella casella di testo **Nome visualizzato** digitare **Britta Simon**.

    d. Nell'elenco **Ruolo** selezionare **Utente**.

    e. Fare clic su **Avanti**.

7. Nella pagina **Ottieni password temporanea** fare clic su **crea**.

    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-jobscore-tutorial/create_aaduser_07.png) 

8. Nella pagina **Ottieni password temporanea** seguire questa procedura:

    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-jobscore-tutorial/create_aaduser_08.png) 

    a. Prendere nota del valore visualizzato in **Nuova password**.

    b. Fare clic su **Completa**.   



### <a name="creating-a-jobscore-test-user"></a>Creazione di un utente test di JobScore

In questa sezione viene creato un utente chiamato Britta Simon in JobScore. Collaborare con il [team di supporto di JobScore](emaiLto:support@jobscore.com) per aggiungere gli utenti alla piattaforma JobScore.


### <a name="assigning-the-azure-ad-test-user"></a>Assegnazione dell'utente test di Azure AD

In questa sezione Britta Simon viene abilitata per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a JobScore.

![Assegna utente][200] 

**Per assegnare Britta Simon a JobScore, seguire questa procedura:**

1. Per aprire la visualizzazione delle applicazioni nel portale classico, nella visualizzazione directory fare clic su **Applicazioni** nel menu in alto.

    ![Assegna utente][201] 

2. Nell'elenco delle applicazioni selezionare **JobScore**.

    ![Configura accesso Single Sign-On](./media/active-directory-saas-jobscore-tutorial/tutorial_jobscore_50.png) 

3. Scegliere **Utenti**dal menu in alto.

    ![Assegna utente][203] 

4. Nell'elenco di utenti selezionare **Britta Simon**.

5. Fare clic su **Assegna**sulla barra degli strumenti in basso.
    
    ![Assegna utente][205]



### <a name="testing-single-sign-on"></a>Test dell'accesso Single Sign-On

In questa sezione viene testata la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.

Quando si fa clic sul riquadro JobScore nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione JobScore.


## <a name="additional-resources"></a>Risorse aggiuntive

* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-jobscore-tutorial/tutorial_general_205.png



<!--HONumber=Dec16_HO1-->


