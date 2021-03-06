---
title: Esercitazione Integrazione di Azure Active Directory con Flatter Files | Documentazione Microsoft
description: Informazioni su come configurare l&quot;accesso Single Sign-On tra Azure Active Directory e Flatter Files.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: f86fe5e3-0e91-40d6-869c-3df6912d27ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 488a3c5f0aa05c5b71bf5d72539cbc4b7c6de1b5
ms.openlocfilehash: 062878ad877b501ce7f0d5c4f8ce9ca939ffe64d
ms.lasthandoff: 02/17/2017


---
# <a name="tutorial-azure-active-directory-integration-with-flatter-files"></a>Esercitazione: Integrazione di Azure Active Directory con Flatter Files
Questa esercitazione descrive l'integrazione di Flatter Files con Azure Active Directory (Azure AD).  

L'integrazione di Flatter Files con Azure AD offre i vantaggi seguenti: 

* È possibile controllare in Azure AD chi può accedere a Flatter Files 
* È possibile abilitare gli utenti per l'accesso Single Sign-On automatico (SSO) a Flatter Files con i propri account Azure AD
* È possibile gestire gli account in una posizione centrale, il portale di Azure Active Directory classico.

Per altre informazioni sull'integrazione di app SaaS con Azure AD, vedere [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Prerequisiti
Per configurare l'integrazione di Azure AD con Flatter Files, sono necessari gli elementi seguenti:

* Sottoscrizione di Azure AD.
* Sottoscrizione di Flatter Files abilitata per l'accesso Single Sign-On

>[!NOTE]
>Non è consigliabile usare un ambiente di produzione per testare i passaggi di questa esercitazione. 
> 

A questo scopo, è consigliabile seguire le indicazioni seguenti:

* Non usare l'ambiente di produzione, a meno che non sia necessario.
* Se non si dispone di un ambiente di prova di Azure AD, è possibile ottenere una versione di valutazione di un mese [qui](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Descrizione dello scenario
L'obiettivo di questa esercitazione è testare l'accesso Single Sign-On di Azure AD in un ambiente di test.  

Lo scenario descritto in questa esercitazione prevede i due blocchi predefiniti seguenti:

1. Aggiunta di Flatter Files dalla raccolta 
2. Configurazione e test dell'accesso Single Sign-On di Azure AD

## <a name="add-flatter-files-from-the-gallery"></a>Aggiungere Flatter Files dalla raccolta
Per configurare l'integrazione di Flatter Files in Azure AD, è necessario aggiungere Flatter Files dalla raccolta al proprio elenco di app SaaS gestite.

**Per aggiungere Flatter Files dalla raccolta, seguire questa procedura:**

1. Nel **portale di Azure classico**fare clic su **Active Directory**nel riquadro di spostamento sinistro. 
   
    ![Active Directory][1]
2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.
3. Per aprire la visualizzazione applicazioni, nella visualizzazione directory fare clic su **Applications** nel menu superiore.
   
    ![Applications][2]
4. Fare clic su **Add** nella parte inferiore della pagina.
   
    ![Applicazioni][3]
5. Nella finestra di dialogo **Come procedere** fare clic su **Aggiungere un'applicazione dalla raccolta**.
   
    ![Applicazioni][4]
6. Nella casella di ricerca digitare **Flatter Files**.

    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_01.png)

7. Nel riquadro dei risultati selezionare **Flatter Files** e quindi fare clic su **Completa** per aggiungere l'applicazione.
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_500.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurare e testare l'accesso Single Sign-On di Azure AD
Questa sezione descrive come configurare e testare l'accesso Single Sign-On di Azure AD con Flatter Files in base a un utente test di nome "Britta Simon".

Per il funzionamento dell'accesso SSO, Azure AD deve conoscere qual è l'utente di Flatter Files che corrisponde a un utente di Azure AD. In altre parole, deve essere stabilita una relazione di collegamento tra un utente di Azure AD e l'utente correlato in Flatter Files.  

La relazione di collegamento viene stabilita assegnando al valore di **nome utente** in Azure AD lo stesso valore di **Username** in Flatter Files.

Per configurare e testare l'accesso Single Sign-On di Azure AD con Flatter Files, è necessario completare i blocchi predefiniti seguenti:

1. **[Configurazione dell'accesso Single Sign-On di Azure AD](#configuring-azure-ad-single-single-sign-on)**: per abilitare gli utenti all'uso di questa funzionalità.
2. **[Creazione di un utente test di Azure AD](#creating-an-azure-ad-test-user)** : per testare l'accesso Single Sign-On di Azure AD con l'utente Britta Simon.
3. **[Creazione di un utente test per Flatter Files](#creating-a-halogen-software-test-user)** : per avere una controparte di Britta Simon in Flatter Files collegata alla relativa rappresentazione in Azure AD.
4. **[Assegnazione dell'utente test di Azure AD](#assigning-the-azure-ad-test-user)** : per abilitare Britta Simon all'uso dell'accesso Single Sign-On di Azure AD.
5. **[Test dell'accesso Single Sign-On](#testing-single-sign-on)** : per verificare se la configurazione funziona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurare l'accesso Single Sign-On di Azure AD
Questa sezione descrive come abilitare l'accesso Single Sign-On di Azure AD nel portale di Azure AD classico e configurare l'accesso Single Sign-On nell'applicazione Flatter Files. Come parte di questa procedura, verrà richiesto di creare un file di certificato con codifica Base&64;. Se non si ha familiarità con questa procedura, vedere il video che illustra [come convertire un certificato binario in un file di testo](http://youtu.be/PlgrzUZ-Y1o).
Per configurare l'accesso Single Sign-On per Flatter Files, è necessario un dominio registrato. Se ancora non si ha un dominio registrato, contattare il team di supporto di Flatter Files all'indirizzo [support@flatterfiles.com](mailto:support@flatterfiles.com).  

**Per configurare l'accesso Single Sign-On di Azure AD con Flatter Files, seguire questa procedura:**

1. Nella pagina di integrazione dell'applicazione **Flatter Files** del portale di Azure AD classico fare clic su **Configura accesso Single Sign-On** per aprire la finestra di dialogo **Configura accesso Single Sign-On**.
   
    ![Configura accesso Single Sign-On][6] 
2. Nella pagina **How would you like users to sign on to Flatter Files** (Stabilire come si desidera che gli utenti accedano a Flatter Files) selezionare **Single Sign-On di Microsoft Azure AD** e quindi fare clic su **Avanti**.
       ![Configura accesso Single Sign-On](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_02.png) 
3. Nella pagina **Configurare le impostazioni dell'app** fare clic su **Avanti**.
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_03.png) 
   >[!NOTE]
   >Flatter Files usa lo stesso URL di accesso SSO per tutti i clienti: [https://www.flatterfiles.com/site/login/sso/](https://www.flatterfiles.com/site/login/sso/).
   > 
   
4. Nella pagina **Configure single sign-on at Flatter Files** (Configura accesso Single Sign-On in Flatter Files) seguire questa procedura:
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_04.png)  
    1. Fare clic su **Scarica certificato**e quindi salvare il file nel computer.
    2. Fare clic su **Avanti**.
5. Accedere all'applicazione Flatter Files come amministratore.
6. Fare clic su Dashboard. 
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_05.png)  
7. Fare clic su **Settings** (Impostazioni) e quindi nella scheda **Company** (Azienda) seguire questa procedura: 
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_06.png)  
    1. Selezionare **Use SAML 2.0 for Authentication**.
    2. Fare clic su **Configure SAML**.
8. Nella finestra di dialogo **SAML Configuration** (Configurazione SAML) seguire questa procedura: 
   
    ![Configura accesso Single Sign-On](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_08.png)  
   1. Nella casella di testo Domain digitare il dominio registrato.
   
    >[!NOTE]
    >Se ancora non si ha un dominio registrato, contattare il team di supporto di Flatter Files all'indirizzo [support@flatterfiles.com] (mailto:support@flatterfiles.com). 
    >    
   2. Nella pagina Configura accesso Single Sign-On in Flatter Files del portale di Azure classico copiare il valore di URL servizio Single Sign-On e quindi incollarlo nella casella di testo Identity Provider URL.
   3.  Creare un file con **codifica Base&64;** dal certificato scaricato.  
 
   >[!TIP]
   >Per informazioni dettagliate, vedere il video che illustra [come convertire un certificato binario in un file di testo](http://youtu.be/PlgrzUZ-Y1o).
   >  
   4.  Aprire il certificato con codifica Base&64; nel Blocco note, copiarne il contenuto negli Appunti e quindi incollarlo nella casella di testo **FlatterFiles Identity Provider Certificate** .
   5. Fare clic su **Update**.
9. Nel portale di Azure AD classico selezionare la conferma della configurazione dell'accesso Single Sign-On e quindi fare clic su **Avanti**. 
   
    ![Single Sign-On di Microsoft Azure AD][10]
10. Nella pagina **Conferma Single Sign-on** fare clic su **Completa**.  
    
     ![Single Sign-On di Microsoft Azure AD][11]

### <a name="create-an-azure-ad-test-user"></a>Creare un utente test di Azure AD
Questa sezione descrive come creare un utente test chiamato Britta Simon nel portale di Azure classico.

![Creare un utente di Azure AD][20]

**Per creare un utente test in Azure AD, eseguire la procedura seguente:**

1. Nel **portale di Azure classico** fare clic su **Active Directory** nel riquadro di spostamento sinistro.
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_09.png) 
2. Nell'elenco **Directory** selezionare la directory per la quale si desidera abilitare l'integrazione delle directory.
3. Per visualizzare l'elenco di utenti, fare clic su **Utenti**nel menu in alto.
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_03.png) 
4. Per aprire la finestra di dialogo **Aggiungi utente**, fare clic su **Aggiungi utente** nella barra degli strumenti in basso. 
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_04.png) 
5. Nella pagina **Informazioni sull'utente** seguire questa procedura: 
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_05.png)  
   1. In Tipo di utente selezionare Nuovo utente nell'organizzazione.
   2. Nella casella di testo **Nome utente** digitare **BrittaSimon**.
   3. Fare clic su **Avanti**.
6. Nella pagina **Profilo utente** seguire questa procedura: 
   
   ![Creazione di un utente test di Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_06.png) 
   1. Nella casella di testo **Nome** digitare **Britta**.  
   2. Nella casella di testo **Cognome** digitare **Simon**.
   3. Nella casella di testo **Nome visualizzato** digitare **Britta Simon**.
   4. Nell'elenco **Ruolo** selezionare **Utente**.
   5. Fare clic su **Avanti**.
7. Nella pagina **Ottieni password temporanea** fare clic su **crea**.
   
    ![Creazione di un utente test di Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_07.png) 
8. Nella pagina **Ottieni password temporanea** seguire questa procedura:
   
   ![Creazione di un utente test di Azure AD](./media/active-directory-saas-flatter-files-tutorial/create_aaduser_08.png) 
   1. Prendere nota del valore visualizzato in **Nuova password**.
   2. Fare clic su **Complete**.   

### <a name="create-a-flatter-files-test-user"></a>Creare un utente test per Flatter Files
Questa sezione descrive come creare un utente chiamato Britta Simon in Flatter Files.

**Per creare un utente denominato Britta Simon in Flatter Files, seguire questa procedura:**

1. Accedere al sito aziendale di **Flatter Files** come amministratore.
2. Nel riquadro di spostamento a sinistra fare clic su **Settings** (Impostazioni) e quindi fare clic sulla scheda **Users** (Utenti).
   
    ![Creazione di un utente in Flatter Files](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_09.png)
3. Fare clic su **Add User**. 
4. Nella finestra di dialogo **Aggiungi utente** seguire questa procedura:
   
    ![Creazione di un utente in Flatter Files](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_10.png)
   1. Nella casella di testo **Nome** digitare **Britta**.
   2. Nella casella di testo **Cognome** digitare **Simon**. 
   3. Nella casella di testo **Email Address** (Indirizzo di posta elettronica) digitare l'indirizzo di posta elettronica di Britta Simon nel portale di Azure classico.
   4. Fare clic su **Submit**.   

### <a name="assign-the-azure-ad-test-user"></a>Assegnare l'utente test di Azure AD
Questa sezione descrive come abilitare Britta Simon per l'uso dell'accesso Single Sign-On di Azure concedendole l'accesso a Flatter Files.

![Assegna utente][200] 

**Per assegnare Britta Simon a Flatter Files, seguire questa procedura:**

1. Per aprire la visualizzazione applicazioni nel portale di Azure classico, nella visualizzazione directory fare clic su **Applicazioni** nel menu in alto.
   
    ![Assegna utente][201] 
2. Nell'elenco di applicazioni selezionare **Flatter Files**.
   
    ![Assegna utente](./media/active-directory-saas-flatter-files-tutorial/tutorial_flatter_files_11.png) 
3. Scegliere **Utenti**dal menu in alto.
   
    ![Assegna utente][203] 
4. Nell'elenco di utenti selezionare **Britta Simon**.
5. Fare clic su **Assegna**sulla barra degli strumenti in basso.
   
    ![Assegna utente][205]

### <a name="test-single-sign-on"></a>Testare l'accesso Single Sign-On
Questa sezione descrive come testare la configurazione dell'accesso Single Sign-On di Azure AD usando il pannello di accesso.  

Quando si fa clic sul riquadro Flatter Files nel pannello di accesso, si dovrebbe accedere automaticamente all'applicazione Flatter Files.

## <a name="additional-resources"></a>Risorse aggiuntive
* [Elenco di esercitazioni sulla procedura di integrazione delle app SaaS con Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Informazioni sull'accesso alle applicazioni e Single Sign-On con Azure Active Directory](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-flatter-files-tutorial/tutorial_general_205.png







