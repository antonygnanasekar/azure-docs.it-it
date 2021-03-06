---
title: Campionare i dati in contenitori BLOB di Azure, SQL Server e nelle tabelle Hive | Documentazione Microsoft
description: Come esplorare i dati archiviati in vari ambienti di Azure.
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 80a9dfae-e3a6-4cfb-aecc-5701cfc7e39d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: fashah;garye;bradsev
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 0683be564a88ef54882e8d882d196851ecac243d
ms.lasthandoff: 04/12/2017


---
# <a name="heading"></a>Campionare i dati in contenitori BLOB di Azure, SQL Server e nelle tabelle Hive
Questo documento include collegamenti ad argomenti che trattano come campionare i dati archiviati in uno fra tre diversi percorsi di Azure:

* **I dati del contenitore BLOB di Azure** vengono campionati scaricandoli a livello di programmazione ed eseguendo il successivo campionamento usando un codice Python di esempio.
* **Dati di SQL Server** vengono campionati utilizzando sia il linguaggio di programmazione Python che SQL. 
* **Dati della tabella hive** vengono campionati utilizzando le query Hive.

Il **menu** seguente contiene collegamenti ad argomenti che descrivono come campionare i dati da ognuno di questi ambienti di archiviazione di Azure. 

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Questo campionamento è un passaggio del [Processo di analisi scientifica dei dati per i team (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

**Perché campionare i dati?**

Se il set di dati da analizzare è grande, è in genere opportuno sottocampionare i dati per ridurlo e ottenere dimensioni inferiori più facilmente gestibili ma comunque rappresentative. Questa operazione facilita la comprensione e l'esplorazione dei dati, nonché la progettazione di funzionalità. Il suo ruolo nel Cortana Analytics Process consiste nell'abilitare la creazione relativa a prototipi di funzioni di elaborazione dei dati e di modelli per l'apprendimento automatico.


