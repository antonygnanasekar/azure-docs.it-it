---
title: Problema nella configurazione dell&quot;accesso Single Sign-On federato per un&quot;applicazione non inclusa nella raccolta di Azure AD | Microsoft Docs
description: Informazioni sui problemi comuni che si possono incontrare durante la configurazione dell&quot;accesso Single Sign-On federato per un&quot;applicazione SAML personalizzata non inclusa nella raccolta delle applicazioni di Azure AD
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: asteen
translationtype: Human Translation
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: 5c4ff9ce7dc2c70496b7f7f25de3ee31bd45a662
ms.lasthandoff: 04/17/2017


---

# <a name="problem-configuring-federated-single-sign-on-for-a-non-gallery-application"></a>Problema nella configurazione dell'accesso Single Sign-On federato per un'applicazione non inclusa nella raccolta di Azure AD

Se si verifica un problema durante la configurazione di un'applicazione. Verificare di avere seguito tutti i passaggi indicati nell'articolo [Configurazione del servizio Single Sign-On in applicazioni non presenti nella raccolta di applicazioni di Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps).

## <a name="cant-add-another-instance-of-the-application"></a>Impossibile aggiungere un'altra istanza dell'applicazione

Per aggiungere una seconda istanza di un'applicazione, è necessario:

-   Configurare un identificatore univoco per la seconda istanza. Non è possibile configurare lo stesso identificatore usato per la prima istanza.

-   Configurare un certificato diverso da quello usato per la prima istanza.

Se l'applicazione non supporta alcuna delle operazioni sopra indicate, non è possibile configurare una seconda istanza.

## <a name="where-do-i-set-the-entityid-user-identifier-format"></a>Dove impostare il formato di EntityID (Identificatore utente)

Non è possibile selezionare il formato di EntityID (Identificatore utente) che Azure AD invia all'applicazione in risposta all'autenticazione dell'utente.

Azure AD seleziona il formato per l'attributo NameID (identificatore utente) in base al valore selezionato o al formato richiesto dall'applicazione nell'oggetto AuthRequest SAML. Per altre informazioni, vedere l'articolo relativo al [protocollo SAML per Single Sign-On](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) sotto la sezione NameIDPolicy.

## <a name="where-do-i-get-the-application-metadata-or-certificate-from-azure-ad"></a>Dove scaricare il certificato o i metadati dell'applicazione da Azure AD

Per scaricare il certificato o i metadati dell'applicazione da Azure AD, seguire questa procedura:

1.  Aprire il [**portale di Azure**](https://portal.azure.com/) e accedere come **Amministratore globale** o **Coamministratore**.

2.  Aprire l'**estensione Azure Active Directory** facendo clic su **Altri servizi** nella parte inferiore del menu di navigazione principale a sinistra.

3.  Digitare "**Azure Active Directory**" nella casella di ricerca filtro e selezionare l'elemento **Azure Active Directory**.

4.  Fare clic su **Applicazioni aziendali** nel menu di navigazione a sinistra di Azure Active Directory.

5.  Fare clic su **Tutte le applicazioni** per visualizzare un elenco di tutte le applicazioni.

   * Se l'applicazione non è inclusa nell'elenco, usare il controllo **Filtro** all'inizio dell'elenco **Tutte le applicazioni** e impostare l'opzione **Mostra** su **Tutte le applicazioni**.

6.  Selezionare l'applicazione per cui è stato configurato l'accesso Single Sign-On.

7.  Dopo il caricamento dell'applicazione, fare clic su **Single Sign-On** nel menu di navigazione a sinistra dell'applicazione.

8.  Passare alla sezione **Certificato di firma SAML** e quindi fare clic sul valore della colonna **Download**. A seconda di quale applicazione richiede la configurazione dell'accesso Single Sign-On, è visibile l'opzione per scaricare il codice XML dei metadati o l'opzione per scaricare il certificato.

Azure AD non fornisce URL per ottenere i metadati. I metadati possono essere recuperati solo come file XML.

## <a name="next-steps"></a>Passaggi successivi
[Gestione di applicazioni con Azure Active Directory](active-directory-enable-sso-scenario.md)

