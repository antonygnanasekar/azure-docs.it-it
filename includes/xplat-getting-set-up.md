---
services: virtual-machines
title: Configurazione dell&quot;interfaccia della riga di comando di Azure per la gestione dei servizi
author: squillace
solutions: 
manager: timlt
editor: tysonn
ms.service: virtual-machine
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: linux
ms.workload: infrastructure
ms.date: 04/13/2015
ms.author: rasquill
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 737e4be21eefcbd6fbd31d15a6f77770cd59ca67
ms.lasthandoff: 03/21/2017


---
## <a name="using-azure-cli"></a>Utilizzare l'interfaccia della riga di comando di Azure
I passaggi seguenti consentono di utilizzare l'interfaccia della riga di comando di Azure facilmente con la versione più recente e la sottoscrizione appropriata. Se è necessario installare l'interfaccia della riga di comando di Azure e connetterla al proprio account, vedere [Interfaccia della riga di comando di Azure](../articles/cli-install-nodejs.md).

### <a name="step-1-update-azure-cli-version"></a>Passaggio 1: aggiornare la versione dell'interfaccia della riga di comando di Azure
Al fine di utilizzare l'interfaccia della riga di comando di Azure per i comandi imperativi con modalità di gestione del servizio, sarebbe opportuno disporre di una versione recente. Per verificare la versione, digitare `azure --version`. Dovrebbe essere visualizzata una schermata analoga alla seguente:

    $ azure --version
    0.8.17 (node: 0.10.25)

Se si desidera aggiornare la versione dell'interfaccia della riga di comando di Azure, vedere [Interfaccia della riga di comando di Azure](https://github.com/Azure/azure-xplat-cli).

### <a name="step-2-set-the-azure-account-and-subscription"></a>Passaggio 2: Impostare l'account e la sottoscrizione di Azure
Una volta connessa l'interfaccia della riga di comando di Azure all'account che si desidera utilizzare, è possibile che siano visualizzate più sottoscrizioni. In questo caso, è consigliabile esaminare le sottoscrizioni disponibili per l'account digitando `azure account list` e quindi selezionando la sottoscrizione che si vuole usare digitando `azure account set <subscription id or name> true` dove *subscription id or name* è l'ID sottoscrizione o il nome della sottoscrizione che si vuole usare nella sessione corrente. Verrà visualizzata una schermata simile alla seguente:

    $ azure account set "Visual Studio Ultimate with MSDN" true
    info:    Executing command account set
    info:    Setting subscription to "Visual Studio Ultimate with MSDN" with id "0e220bf6-5caa-4e9f-8383-51f16b6c109f".
    info:    Changes saved
    info:    account set command OK

> [!NOTE]
> Se non è già un account Azure, ma è una sottoscrizione di abbonamento MSDN, è possibile ottenere i crediti di Azure attivando il [abbonati MSDN dei vantaggi qui](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) oppure è possibile utilizzare l'account gratuito. Uno funzionerà per l'accesso ad Azure.
> 
> 


