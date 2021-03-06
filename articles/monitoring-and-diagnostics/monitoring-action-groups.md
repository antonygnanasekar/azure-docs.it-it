---
title: Creare e gestire gruppi di azione nel portale di Azure | Microsoft Docs
description: 
author: anirudhcavale
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: ancav
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 9a290422bf897397942fccf2e3733d0709701e91
ms.lasthandoff: 03/31/2017


---
# <a name="create-and-manage-action-groups-in-azure-portal"></a>Creare e gestire gruppi di azione nel portale di Azure
## <a name="overview"></a>Panoramica ##
Questo articolo illustra come creare e gestire gruppi di azione nel portale di Azure.

I gruppi di azione consentono di configurare un elenco di destinatari. Questi gruppi possono essere successivamente impiegati quando si definiscono gli avvisi del log attività per garantire che un determinato gruppo di azione riceva una notifica quando viene generato l'avviso del log attività.

Un gruppo di azione può avere fino a 10 tipi di azioni. Un'azione è definita dalla combinazione di:

**Nome:** un identificatore univoco all'interno del gruppo di azione.  
**Tipo di azione:** definisce l'azione che verrà eseguita. Le opzioni sono inviare SMS, inviare posta elettronica o chiamare un webhook.  
**Dettagli:** in base al tipo di azione è necessario fornire un numero di telefono, un indirizzo di posta elettronica o l'URI del webhook.

È possibile configurare e ottenere informazioni sugli avvisi di notifica dello stato del servizio usando:
* [portale di Azure](monitoring-action-groups.md)
- [Modelli di Resource Manager](monitoring-create-action-group-with-resource-manager-template.md)

## <a name="creating-an-action-group-for-the-azure-portal"></a>Creazione di un gruppo di azione per il portale di Azure ##
1.    Nel [portale](https://portal.azure.com), passare al servizio **Monitoraggio**

    ![Monitoraggio](./media/monitoring-action-groups/home-monitor.png)
2.    Fare clic sull'opzione **Monitoraggio** per aprire il relativo pannello che riunisce tutte le impostazioni e i dati di monitoraggio in un'unica vista consolidata. Per prima cosa si apre la sezione **Log di attività**.

3.    Fare clic sulla sezione **Gruppi di azione**

    ![Gruppo di azione](./media/monitoring-action-groups/action-groups-blade.png)
4.    Fare clic sul comando **Aggiungi** gruppo di azione e compilare i campi

    ![Aggiungi-gruppo-azione](./media/monitoring-action-groups/add-action-group.png)
5.    Fornire un **nome** e un **nome breve** per il gruppo di azione. Al nome breve verrà fatto riferimento nelle notifiche inviate a questo gruppo

      ![Definizione-gruppo-azione](./media/monitoring-action-groups/action-group-define.png)

6.    Il gruppo di azione verrà salvato in **Sottoscrizione**. Verrà compilato automaticamente nella sottoscrizione attualmente in uso.

7.    Scegliere il **Gruppo di risorse** per questo gruppo di azione.

8.    Quindi, definire un elenco di azioni tramite una combinazione di:
  1. **Nome:** un identificatore univoco all'interno del gruppo di azione.
  2. **Tipo di azione:** definisce l'azione che verrà eseguita. Le opzioni sono inviare SMS, inviare posta elettronica o chiamare un webhook.
  3. **Dettagli:** in base al tipo di azione è necessario fornire un numero di telefono, un indirizzo di posta elettronica o l'URI del webhook.

9.    Al termine fare clic su **OK** per creare il gruppo di azione.

## <a name="managing-your-action-groups"></a>Gestione dei gruppi di azione ##
Dopo aver creato un gruppo di azione, esso sarà visibile nella sezione dei gruppi di azione del servizio di monitoraggio. Selezionare il gruppo di azione che si desidera gestire, per:
* Aggiungere, modificare o rimuovere destinatari.
-    Eliminare il gruppo di azione.

## <a name="next-steps"></a>Passaggi successivi: ##
Altre informazioni sul [comportamento degli avvisi SMS](monitoring-sms-alert-behavior.md)  
[Comprendere lo schema webhook degli avvisi del log attività](monitoring-activity-log-alerts-webhook.md)  
Altre informazioni sulla [limitazione della frequenza](monitoring-alerts-rate-limiting.md) degli avvisi  
Ottenere una [panoramica degli avvisi del log attività](monitoring-overview-alerts.md) e informazioni sulla ricezione degli avvisi  
Come [configurare gli avvisi ogni volta che viene inviata una notifica di integrità del servizio](monitoring-activity-log-alerts-on-service-notifications.md)

