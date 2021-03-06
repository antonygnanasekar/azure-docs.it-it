---
title: Log e diagnostica per ASP.NET in Azure Application Insights | Documentazione Microsoft
description: Diagnosticare i problemi nelle app Web ASP.NET eseguendo ricerche in richieste, eccezioni e log generati con Trace, NLog or Log4Net.
services: application-insights
documentationcenter: 
author: alancameronwills
manager: douge
ms.assetid: 99860c53-0324-4a3a-9aa9-83f5dffba835
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/08/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 08ce387dd37ef2fec8f4dded23c20217a36e9966
ms.openlocfilehash: 874e9abb7ae7e06808645ae2ab7cd5b3c0d36e04
ms.lasthandoff: 01/25/2017


---
# <a name="logs-exceptions-and-custom-diagnostics-for-aspnet-in-application-insights"></a>Log, eccezioni e diagnostica personalizzata per ASP.NET in Application Insights
[Application Insights][start] include un potente strumento di [ricerca diagnostica][diagnostic] che consente di esplorare e analizzare i dati di telemetria inviati da Application Insights SDK dall'applicazione. Molti eventi come le visualizzazioni pagina dell'utente vengono inviate automaticamente dall'SDK.

È anche possibile scrivere il codice per inviare eventi personalizzati, report di eccezioni e tracce. E se si usa già un framework di registrazione, ad esempio log4J, log4net, NLog o System.Diagnostics.Trace, è possibile acquisire tali log e includerli nella ricerca. Ciò semplifica la correlazione delle tracce dei log con azioni utente, eccezioni e altri eventi.

## <a name="send"></a>Prima di scrivere i dati di telemetria personalizzati
Se non si è ancora [configurato Application Insights per il progetto][start], è possibile farlo ora.

Quando si esegue l'applicazione, vengono inviati alcuni dati di telemetria che vengono visualizzati nella ricerca diagnostica, tra cui le richieste ricevute dal server, le visualizzazioni pagina registrate nel client e le eccezioni non rilevate.

Aprire Ricerca diagnostica per visualizzare i dati di telemetria inviati automaticamente dall'SDK.

![](./media/app-insights-search-diagnostic-logs/appinsights-45diagnostic.png)

![](./media/app-insights-search-diagnostic-logs/appinsights-31search.png)

I dettagli variano da un tipo di applicazione all'altro. È possibile fare clic su un singolo evento per ottenere altri dettagli.

## <a name="sampling"></a>Campionamento
Se l'applicazione invia una grande quantità di dati ed è in uso Application Insights SDK per ASP.NET 2.0.0 Beta3 o versioni successive, la funzionalità del campionamento adattivo può operare e inviare solo una percentuale dei dati di telemetria. [Altre informazioni sul campionamento.](app-insights-sampling.md)

## <a name="events"></a>Eventi personalizzati
Gli eventi personalizzati vengono visualizzati in [Diagnostic Search][diagnostic] (Ricerca diagnostica) e in [Esplora metriche][metrics]. È possibile inviarli da dispositivi, pagine web e applicazioni server. Possono essere usati per scopi diagnostici e per [comprendere i criteri di utilizzo][track].

Un evento personalizzato è contraddistinto da un nome e può contenere anche proprietà che è possibile filtrare, insieme alle misurazioni numeriche.

JavaScript nel client

    appInsights.trackEvent("WinGame",
         // String properties:
         {Game: currentGame.name, Difficulty: currentGame.difficulty},
         // Numeric measurements:
         {Score: currentGame.score, Opponents: currentGame.opponentCount}
         );

C# nel server

    // Set up some properties:
    var properties = new Dictionary <string, string> 
       {{"game", currentGame.Name}, {"difficulty", currentGame.Difficulty}};
    var measurements = new Dictionary <string, double>
       {{"Score", currentGame.Score}, {"Opponents", currentGame.OpponentCount}};

    // Send the event:
    telemetry.TrackEvent("WinGame", properties, measurements);


VB nel server

    ' Set up some properties:
    Dim properties = New Dictionary (Of String, String)
    properties.Add("game", currentGame.Name)
    properties.Add("difficulty", currentGame.Difficulty)

    Dim measurements = New Dictionary (Of String, Double)
    measurements.Add("Score", currentGame.Score)
    measurements.Add("Opponents", currentGame.OpponentCount)

    ' Send the event:
    telemetry.TrackEvent("WinGame", properties, measurements)

### <a name="run-your-app-and-view-the-results"></a>Eseguire l'app e visualizzare i risultati.
Aprire Ricerca diagnostica.

Selezionare Evento personalizzato e selezionare un nome di evento specifico.

![](./media/app-insights-search-diagnostic-logs/appinsights-332filterCustom.png)

Filtrare i dati ulteriormente immettendo un termine di ricerca per un valore della proprietà.  

![](./media/app-insights-search-diagnostic-logs/appinsights-23-customevents-5.png)

Analizzare un singolo evento per visualizzarne le proprietà dettagliate.

![](./media/app-insights-search-diagnostic-logs/appinsights-23-customevents-4.png)

## <a name="pages"></a> Visualizzazioni pagina
La telemetria delle visualizzazioni pagina viene inviata dalla chiamata a trackPageView() nel [frammento di codice JavaScript inserito nelle pagine Web][usage]. Lo scopo principale è quello di contribuire al conteggio delle visualizzazioni pagina che si verificano nella pagina di panoramica.

In genere viene eseguita una sola chiamata in ogni pagina HTML, ma è possibile inserire ulteriori chiamate, ad esempio se si ha un'app di una sola pagina e si vuole registrare una nuova pagina ogni volta che l'utente ottiene altri dati.

    appInsights.trackPageView(pageSegmentName, "http://fabrikam.com/page.htm"); 

A volte è utile collegare le proprietà che è possibile usare come filtri nella ricerca di diagnostica:

    appInsights.trackPageView(pageSegmentName, "http://fabrikam.com/page.htm",
     {Game: currentGame.name, Difficulty: currentGame.difficulty});


## <a name="trace"></a> Telemetria di traccia
La telemetria di traccia rappresenta il codice che viene inserito specificamente per creare i log di diagnostica. 

È, ad esempio, possibile inserire chiamate simili alla seguente:

    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow response - database01");


#### <a name="install-an-adapter-for-your-logging-framework"></a>Installare un adattatore per il framework di registrazione
È anche possibile eseguire la ricerca nei log generati con un framework di registrazione come log4Net, NLog o System.Diagnostics.Trace. 

1. Se si intende usare log4Net o NLog, installarlo nel progetto. 
2. In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e scegliere **Gestisci pacchetti NuGet**.
3. Selezionare Online > Tutti, quindi selezionare **Includi versione preliminare** e cercare "Microsoft.ApplicationInsights".
   
    ![Ottenere la versione preliminare dell'adattatore appropriato](./media/app-insights-search-diagnostic-logs/appinsights-36nuget.png)
4. Selezionare il pacchetto appropriato tra:
   
   * Microsoft.ApplicationInsights.TraceListener (per acquisire le chiamate System.Diagnostics.Trace)
   * Microsoft.ApplicationInsights.NLogTarget
   * Microsoft.ApplicationInsights.Log4NetAppender

Il pacchetto NuGet installa gli assembly necessari e modifica inoltre web.config o app.config.

#### <a name="pepper"></a>Inserire chiamate di log di diagnostica
Se si usa System.Diagnostics.Trace, una tipica chiamata sarà simile alla seguente:

    System.Diagnostics.Trace.TraceWarning("Slow response - database01");

Se si preferisce log4net o NLog:

    logger.Warn("Slow response - database01");

Eseguire l'app in modalità debug o distribuirla.

I messaggi verranno visualizzati in Ricerca diagnostica quando si seleziona il filtro di traccia.

### <a name="exceptions"></a>Eccezioni
La visualizzazione dei report delle eccezioni in Application Insights è molto utile, soprattutto perché è possibile spostarsi tra le richieste non riuscite e le eccezioni e leggere lo stack dell'eccezione.

In alcuni casi, è necessario [inserire alcune righe di codice][exceptions] per fare in modo che le eccezioni vengano rilevate automaticamente.

È anche possibile scrivere codice esplicito per inviare dati di telemetria per le eccezioni:

JavaScript

    try 
    { ...
    }
    catch (ex)
    {
      appInsights.TrackException(ex, "handler loc",
        {Game: currentGame.Name, 
         State: currentGame.State.ToString()});
    }

C#

    var telemetry = new TelemetryClient();
    ...
    try 
    { ...
    }
    catch (Exception ex)
    {
       // Set up some properties:
       var properties = new Dictionary <string, string> 
         {{"Game", currentGame.Name}};

       var measurements = new Dictionary <string, double>
         {{"Users", currentGame.Users.Count}};

       // Send the exception telemetry:
       telemetry.TrackException(ex, properties, measurements);
    }

VB

    Dim telemetry = New TelemetryClient
    ...
    Try
      ...
    Catch ex as Exception
      ' Set up some properties:
      Dim properties = New Dictionary (Of String, String)
      properties.Add("Game", currentGame.Name)

      Dim measurements = New Dictionary (Of String, Double)
      measurements.Add("Users", currentGame.Users.Count)

      ' Send the exception telemetry:
      telemetry.TrackException(ex, properties, measurements)
    End Try

I parametri delle proprietà e delle misurazioni sono facoltativi, ma sono utili per filtrare e aggiungere altre informazioni. Ad esempio, per un'app in grado di eseguire diversi giochi, è possibile trovare tutti i report di eccezione correlati a un gioco specifico. È possibile aggiungere qualsiasi numero di elementi a ogni dizionario.

#### <a name="viewing-exceptions"></a>Visualizzazione delle eccezioni
Nel pannello Panoramica viene visualizzato un riepilogo delle eccezioni segnalate ed è possibile fare clic sulle singole eccezioni per visualizzare altri dettagli. ad esempio:

![](./media/app-insights-search-diagnostic-logs/appinsights-039-1exceptions.png)[]

Fare clic su qualsiasi tipo di eccezione per visualizzare occorrenze specifiche:

![](./media/app-insights-search-diagnostic-logs/appinsights-333facets.png)[]

È anche possibile aprire direttamente Ricerca diagnostica, filtrare le eccezioni e scegliere il tipo di eccezione che si vuole visualizzare.

### <a name="reporting-unhandled-exceptions"></a>Segnalazione di eccezioni non gestite
Quando possibile, Application Insights segnala le eccezioni non gestite restituite da dispositivi, [Web browser][usage] o server Web, indipendentemente dal fatto che siano instrumentati da [Status Monitor][redfield] o [Application Insights SDK][greenbrown]. 

Tuttavia, in alcuni casi non è sempre possibile eseguire questa operazione perché .NET Framework rileva le eccezioni.  Per fare in modo che vengano visualizzate tutte le eccezioni, è pertanto necessario scrivere un semplice gestore di eccezioni. La procedura migliore dipende dalla tecnologia. Per altre informazioni, vedere [Telemetria delle eccezioni per ASP.NET][exceptions]. 

### <a name="correlating-with-a-build"></a>Correlazione con una compilazione
Quando si leggono i log di diagnostica, è probabile che il codice sorgente sia stato modificato rispetto a quando è stato distribuito il codice attivo.

È pertanto utile inserire le informazioni di compilazione, ad esempio l'URL della versione corrente, in una proprietà insieme a ogni eccezione o a una traccia. 

Invece di aggiungere la proprietà separatamente a ogni chiamata di eccezione, è possibile impostare le informazioni nel contesto predefinito. 

    // Telemetry initializer class
    public class MyTelemetryInitializer : ITelemetryInitializer
    {
        public void Initialize (ITelemetry telemetry)
        {
            telemetry.Properties["AppVersion"] = "v2.1";
        }
    }

Nell'inizializzatore di app, ad esempio Global.asax.cs, procedere come segue:

    protected void Application_Start()
    {
        // ...
        TelemetryConfiguration.Active.TelemetryInitializers
        .Add(new MyTelemetryInitializer());
    }

### <a name="requests"></a> Richieste del server Web
I dati di telemetria delle richieste vengono inviati automaticamente quando si [installa Status Monitor nel server Web][redfield] o quando si [aggiunge Application Insights al progetto Web][greenbrown]. Vengono inoltre aggiunti nei grafici dei tempi di richiesta e di risposta in Esplora metriche e nella pagina Panoramica.

Se si vogliono inviare eventi aggiuntivi, è possibile usare l'API TrackRequest().

## <a name="questions"></a>Domande e risposte
### <a name="emptykey"></a>Viene visualizzato l'errore: "La chiave di strumentazione non può essere vuota"
Risulta che l'utente abbia installato il pacchetto NuGet dell'adattatore di registrazione senza aver installato Application Insights.

In Esplora soluzioni fare clic con il pulsante destro del mouse su `ApplicationInsights.config` e scegliere **Aggiorna Application Insights**. Verrà visualizzata una finestra di dialogo che invita ad accedere ad Azure e a creare una risorsa di Application Insights o a riusarne una esistente. Il problema verrà in tal modo risolto.

### <a name="limits"></a>Quanti dati vengono conservati?
Fino a 500 eventi al secondo da ciascuna applicazione. Gli eventi vengono conservati per sette giorni.

### <a name="some-of-my-events-or-traces-dont-appear"></a>Alcune delle tracce o degli eventi dell’utente non vengono visualizzati
Se l'applicazione invia una grande quantità di dati ed è in uso Application Insights SDK per ASP.NET 2.0.0 Beta3 o versioni successive, la funzionalità del campionamento adattivo può operare e inviare solo una percentuale dei dati di telemetria. [Altre informazioni sul campionamento.](app-insights-sampling.md)

## <a name="add"></a>Passaggi successivi
* [Configurare i test di disponibilità e velocità di risposta][availability]
* [Risoluzione dei problemi][qna]

<!--Link references-->

[availability]: app-insights-monitor-web-app-availability.md
[diagnostic]: app-insights-diagnostic-search.md
[exceptions]: app-insights-asp-net-exceptions.md
[greenbrown]: app-insights-asp-net.md
[metrics]: app-insights-metrics-explorer.md
[qna]: app-insights-troubleshoot-faq.md
[redfield]: app-insights-monitor-performance-live-website-now.md
[start]: app-insights-overview.md
[track]: app-insights-api-custom-events-metrics.md
[usage]: app-insights-javascript.md



