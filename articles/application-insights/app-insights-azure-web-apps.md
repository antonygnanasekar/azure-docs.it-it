---
title: Monitorare le prestazioni dell&quot;app Web di Azure | Microsoft Docs
description: Monitoraggio delle prestazioni applicative per le app Web di Azure. Tempo di caricamento e risposta del grafico, informazioni sulle dipendenze e impostazione di avvisi sulle prestazioni.
services: application-insights
documentationcenter: .net
author: alancameronwills
manager: carmonm
ms.assetid: 0b2deb30-6ea8-4bc4-8ed0-26765b85149f
ms.service: azure-portal
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/30/2017
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 0c4554d6289fb0050998765485d965d1fbc6ab3e
ms.openlocfilehash: c6f25b8cf8c133f44644db1507958b2176efa230
ms.lasthandoff: 04/13/2017


---
# <a name="monitor-azure-web-app-performance"></a>Monitoraggio delle prestazioni dell'applicazione web di Azure
Nel [portale di Azure](https://portal.azure.com) è possibile configurare il monitoraggio delle prestazioni applicative per le [App Web di Azure](../app-service-web/app-service-web-overview.md). [Application Insights di Azure](app-insights-overview.md) consente di instrumentare l'app per inviare dati di telemetria sulle proprie attività al servizio Application Insights, in cui verranno archiviati e analizzati. Sarà quindi possibile usare grafici delle metriche e strumenti di ricerca per diagnosticare i problemi, migliorare le prestazioni e valutare l'utilizzo.

## <a name="run-time-or-build-time"></a>Fase di esecuzione o fase di compilazione
È possibile configurare il monitoraggio instrumentando l'app in uno dei due modi seguenti:

* **Fase di esecuzione** : è possibile selezionare un'estensione di monitoraggio delle prestazioni quando l'app Web è già attiva. Non è necessario ricompilarla o reinstallarla. Si ottiene un set di pacchetti standard che monitorano i tempi di risposta, le percentuali di riuscita, le eccezioni, le dipendenze e così via. 
* **Fase di compilazione** : è possibile installare un pacchetto nell'app durante lo sviluppo. Questa opzione è più versatile. Oltre agli stessi pacchetti standard, è possibile scrivere codice per personalizzare la telemetria o per inviare dati di telemetria personalizzati. È possibile registrare attività specifiche o registrare eventi in base alla semantica del dominio dell'app. 

## <a name="run-time-instrumentation-with-application-insights"></a>Strumentazione della fase di esecuzione con Application Insights
Se si esegue già un'App Web in Azure, vengono già visualizzati alcuni dati di monitoraggio, cioè la frequenza di esecuzione con errori e la frequenza delle richieste. Aggiungere Application Insights per ottenere più dati, come i tempi di risposta, il monitoraggio delle chiamate alle dipendenze, il rilevamento intelligente e il potente linguaggio di query di Analisi. 

1. **Selezionare Application Insights** nel pannello di controllo di Azure per l'App Web.
   
    ![In Monitoraggio scegliere Application Insights](./media/app-insights-azure-web-apps/05-extend.png)
   
   * Scegliere di creare una nuova risorsa, a meno che non sia già stata impostata una risorsa di Application Insights per l'app da un'altra route.
2. **Instrumentare l'App Web** dopo l'installazione di Application Insights. 
   
    ![Instrumentazione dell'App Web](./media/app-insights-azure-web-apps/restart-web-app-for-insights.png)
3. **Monitorare l'app**.  [Esplorare i dati](#explore-the-data).

In un secondo momento, se si desidera è possibile compilare e ridistribuire l'app con Application Insights.

*Come è possibile rimuovere Application Insights o passare all'invio di un'altra risorsa?*

* In Azure aprire il pannello di controllo dell'App Web e aprire **Estensioni** in Strumenti di sviluppo. Eliminare l'estensione di Application Insights. Quindi in Monitoraggio scegliere Application Insights e creare o selezionare la risorsa desiderata.

## <a name="build-the-app-with-application-insights"></a>Compilare l'app con Application Insights
Application Insights può fornire ulteriori dati di telemetria installando un SDK nell'applicazione. In particolare, è possibile raccogliere i log di traccia, [scrivere dati di telemetria personalizzati](app-insights-api-custom-events-metrics.md) e ottenere report di eccezione più dettagliati.

1. **In Visual Studio** 2013 Update 2 o versione successiva configurare Application Insights per il progetto.

    Fare clic con il pulsante destro del mouse sul progetto Web e scegliere **Configura Application Insights** o **Aggiungi > Application Insights**.
   
    ![Fare clic con il pulsante destro del mouse sul progetto Web e scegliere Aggiungi o Configura Application Insights](./media/app-insights-azure-web-apps/03-add.png)
   
    Se viene chiesto di effettuare l'accesso, usare le credenziali dell'account Azure.
   
    L'operazione ha due effetti:
   
   1. Crea una risorsa di Application Insights in Azure, in cui vengono archiviati, analizzati e visualizzati i dati di telemetria.
   2. Se non è già presente, aggiunge il pacchetto NuGet di Application Insights al codice e lo configura per l'invio della telemetria alla risorsa di Azure.
2. **Testare i dati di telemetria** eseguendo l'app nel computer di sviluppo (F5).
3. **Pubblicare l'app** in Azure nel modo consueto. 

*Come è possibile passare all'invio a un'altra risorsa di Application Insights?*

* In Visual Studio fare clic con il pulsante destro del mouse sul progetto, scegliere **Configura Application Insights** e scegliere la risorsa desiderata. Sarà possibile creare una nuova risorsa. Ricompilare e ridistribuire.

## <a name="explore-the-data"></a>Esplorare i dati
1. In Application Insights, nel pannello di controllo dell'App Web, vengono visualizzate le metriche in tempo reale: ciò significa che le richieste e gli errori vengono mostrati uno o due secondi dopo che si verificano. È molto utile visualizzare questi dati quando si esegue di nuovo la pubblicazione di un'app, perché eventuali problemi sono immediatamente visibili.
2. Fare clic per visualizzare la risorsa Application Insights completa.

    ![Fare clic](./media/app-insights-azure-web-apps/view-in-application-insights.png)

    È anche possibile accedervi direttamente dal riquadro di esplorazione delle risorse di Azure.

1. Fare clic su qualsiasi grafico per visualizzare altri dettagli:
   
    ![Nel pannello di panoramica di Application Insights, fare clic su un grafico](./media/app-insights-azure-web-apps/07-dependency.png)
   
    È possibile [personalizzare i pannelli delle metriche](app-insights-metrics-explorer.md).
2. Fare ancora clic per visualizzare i singoli eventi e le relative proprietà:
   
    ![Fare clic su un tipo di evento per aprire una ricerca filtrata su tale tipo](./media/app-insights-azure-web-apps/08-requests.png)
   
    Si noti il collegamento "…" per aprire tutte le proprietà.
   
    È possibile [personalizzare le ricerche](app-insights-diagnostic-search.md).

Per le ricerche più avanzate nei dati di telemetria, usare il [linguaggio di query di Analisi](app-insights-analytics-tour.md).

## <a name="more-telemetry"></a>Altri dati di telemetria

* [Dati di caricamento della pagina Web](app-insights-javascript.md)
* [Telemetria personalizzata](app-insights-api-custom-events-metrics.md)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Passaggi successivi
* [Eseguire il profiler sull'app live](app-insights-profiler.md).
* [Abilitare l'invio dei dati di diagnostica di Azure](app-insights-azure-diagnostics.md) ad Application Insights.
* [Monitorare le metriche di integrità del servizio](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) per assicurarsi che il servizio sia disponibile e reattivo.
* [Ricevere notifiche di avviso](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) ogni volta che si verificano eventi operativi o le metriche superano una soglia.
* Usare [Analisi dell'utilizzo per applicazioni Web con Application Insights](app-insights-javascript.md) per ottenere i dati di telemetria dei client dai browser che visitano una pagina Web.
* [Monitorare la disponibilità e la velocità di risposta dei siti Web](app-insights-monitor-web-app-availability.md) per ricevere un avviso se il sito è inattivo.


