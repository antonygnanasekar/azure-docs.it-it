---
title: Introduzione all&quot;hop successivo in Azure Network Watcher | Microsoft Docs
description: "Questa pagina fornisce una panoramica della funzionalità hop successivo di Network Watcher"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: febf7bca-e0b7-41d5-838f-a5a40ebc5aac
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 2f03ba60d81e97c7da9a9fe61ecd419096248763
ms.openlocfilehash: 864185e62fb6c3cef4116824b36ee7e5d3447662
ms.lasthandoff: 03/04/2017

---

# <a name="introduction-to-next-hop-in-azure-network-watcher"></a>Introduzione all'hop successivo in Azure Network Watcher

Il traffico proveniente da una macchina virtuale viene inviato a una destinazione in base alle route valide associate alla scheda di interfaccia di rete. L'hop successivo ottiene il tipo di hop successivo e l'indirizzo IP di un pacchetto da una macchina virtuale e una scheda di interfaccia di rete specifiche. Ciò consente di determinare se il pacchetto viene indirizzato a destinazione o se il traffico viene inviato a un black hole. Una configurazione errata delle route da parte dell'utente, in cui il traffico viene indirizzato a un percorso locale oppure a un dispositivo virtuale, può causare problemi di connettività. L'hop successivo restituisce anche la tabella di route associata all'hop successivo. Quando si eseguono query su un hop successivo, viene restituita la route, se definita dall'utente. In caso contrario l'hop successivo restituisce la route di sistema.

![Panoramica dell'hop successivo][1]

Di seguito è riportato un elenco dei tipi di hop successivo che possono essere restituiti dalle query sull'hop successivo.

* Internet
* VirtualAppliance
* VirtualNetworkGateway
* VnetLocal
* HyperNetGateway
* VnetPeering
* None

### <a name="next-steps"></a>Passaggi successivi

Per informazioni su come usare l'hop successivo per rilevare i problemi di connettività di rete, vedere l'articolo relativo alla [verifica dell'hop successivo in una macchina virtuale](network-watcher-check-next-hop-portal.md).

<!--Image references-->
[1]: ./media/network-watcher-next-hop-overview/figure1.png














