---
title: Condivisione esterna di Office 365 e Collaborazione B2B di Azure Active Directory | Documentazione Microsoft
description: Informazioni sul mapping delle attestazioni per Collaborazione B2B in Azure Active Directory
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 02/06/2017
ms.author: sasubram
translationtype: Human Translation
ms.sourcegitcommit: 6c8d7dc7d070ce65cbe637359bf7933eac68ce23
ms.openlocfilehash: ae87c3487abd194c524d067a2a9b7bb9f8df8ac7
ms.lasthandoff: 02/24/2017


---

# <a name="office-365-external-sharing-and-azure-active-directory-b2b-collaboration"></a>Condivisione esterna di Office 365 e Collaborazione B2B di Azure Active Directory

La condivisione esterna in Office 365, con OneDrive, SharePoint Online, gruppi unificati e così via, e in Collaborazione Business-to-Business (B2B) di Azure Active Directory (Azure AD) si equivalgono tecnicamente. Tutti i tipi di condivisione esterna, ad eccezione di OneDrive/SharePoint Online, inclusi gli utenti guest in gruppi unificati, fanno già uso delle API di invito di Collaborazione B2B di Azure AD per la condivisione.

La gestione degli inviti di OneDrive/SharePoint Online è separata. Il supporto per la condivisione esterna in OneDrive/SharePoint Online è iniziato prima dello sviluppo del supporto di Azure AD. Nel tempo la condivisione esterna di OneDrive/SharePoint Online ha accumulato diverse funzionalità e milioni di utenti, che fanno uso del modello di condivisione incorporato nel prodotto. Si sta lavorando su OneDrive/SharePoint Online per aggiungere le API di invito B2B di Azure AD, a cui si fa riferimento in questa documentazione, per unificare il processo tra i prodotti e adottare tutte le innovazioni disponibili con Azure AD. Nel frattempo, rimangono alcune sottili differenze di funzionamento tra la condivisione esterna di OneDrive/SharePoint Online e quella di Collaborazione B2B di Azure AD:

- OneDrive/SharePoint consente di aggiungere utenti alla directory dopo che gli utenti hanno riscattato gli inviti. Prima del riscatto l'utente non è quindi visibile nel portale di Azure AD. Se nel frattempo un altro sito invita un utente, viene generato un nuovo invito. Quando invece si usa Collaborazione B2B di Azure AD, gli utenti vengono aggiunti immediatamente all'invito e sono quindi visibili ovunque.

- L'esperienza di riscatto in OneDrive/SharePoint Online è diversa da quella in Collaborazione B2B di Azure Active Directory. Dopo che un utente ha riscattato un invito, le esperienze sono simili.

- Gli utenti invitati di Collaborazione B2B di Azure AD possono essere selezionati nelle finestre di dialogo di condivisione di OneDrive/SharePoint Online. Dopo avere riscattato gli inviti, gli utenti invitati di OneDrive/SharePoint Online vengono visualizzati anche in Azure AD.

- Per usare la condivisione esterna in OneDrive/SharePoint Online, insieme a Collaborazione B2B di Azure AD in modo gestito e controllato da un amministratore, impostare la condivisione esterna di OneDrive/SharePoint Online in modo da **consentire la condivisione solo con gli utenti esterni già presenti nella directory**. Gli utenti possono accedere a siti condivisi esternamente e scegliere tra i collaboratori esterni che l'amministratore ha aggiunto. L'amministratore può aggiungere i collaboratori esterni tramite le API di invito di Collaborazione B2B.

![Impostazione relativa alla condivisione esterna di Online OneDrive/SharePoint](media/active-directory-b2b-o365-external-user/odsp-sharing-setting.png)

## <a name="next-steps"></a>Passaggi successivi

Vedere gli altri articoli su Azure AD B2B Collaboration.

* [Che cos'è Azure AD B2B Collaboration?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Proprietà dell'utente di Collaborazione B2B](active-directory-b2b-user-properties.md)
* [Aggiunta di un utente di Collaborazione B2B a un ruolo](active-directory-b2b-add-guest-to-role.md)
* [Delegare gli inviti a Collaborazione B2B](active-directory-b2b-delegate-invitations.md)
* [Gruppi dinamici e Collaborazione B2B](active-directory-b2b-dynamic-groups.md)
* [Codici ed esempi di PowerShell per Collaborazione B2B](active-directory-b2b-code-samples.md)
* [Configurare app SaaS per Collaborazione B2B](active-directory-b2b-configure-saas-apps.md)
* [Token utente in Collaborazione B2B](active-directory-b2b-user-token.md)
* [Mapping delle attestazioni utente per Collaborazione B2B](active-directory-b2b-claims-mapping.md)
* [Limitazioni correnti di Collaborazione B2B](active-directory-b2b-current-limitations.md)

