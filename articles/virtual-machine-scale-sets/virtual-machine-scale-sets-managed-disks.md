---
title: "Uso dei dischi gestiti con i set di scalabilità delle macchine virtuali di Azure | Microsoft Docs"
description: "Informazioni su come e perché usare dischi gestiti con i set di scalabilità delle macchine virtuali"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 2/21/2017
ms.author: negat
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: 4ec20a30f423d4b30e953f873100be90167f987b
ms.lasthandoff: 03/29/2017


---
# <a name="azure-vm-scale-sets-and-managed-disks"></a>Set di scalabilità VM di Azure e dischi gestiti

I [set di scalabilità di macchine virtuali](/azure/virtual-machine-scale-sets/) di Azure supportano ora macchine virtuali con dischi gestiti. L'uso di dischi gestiti con i set di scalabilità presenta diversi vantaggi, tra cui:

* Non è più necessario creare in anticipo e gestire gli account di archiviazione per archiviare i dischi del sistema operativo per i set di scalabilità delle macchine virtuali.

* È possibile collegare dischi dati gestiti al set di scalabilità.

* Grazie al disco gestito, un set di scalabilità può contenere fino a un massimo di 1.000 macchine virtuali se basato su un'immagine di piattaforma o di 100 macchine virtuali se basato su un'immagine personalizzata.

## <a name="get-started"></a>Introduzione

Un modo semplice per iniziare a usare i set di scalabilità con i dischi gestiti consiste nel distribuirne uno dal Portale di Azure. Per altre informazioni, vedere [questo articolo](./virtual-machine-scale-sets-portal-create.md). Un altro modo semplice per iniziare è usare l'[interfaccia della riga di comando di Azure 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) per distribuire un set di scalabilità. Nell'esempio seguente viene illustrato come creare un set di scalabilità basato su Ubuntu con 10 macchine virtuali, ognuna con un disco dati da 50 e da 100 GB:

```azurecli
az group create -l southcentralus -n dsktest
az vmss create -g dsktest -n dskvmss --image ubuntults --instance-count 10 --data-disk-sizes-gb 50 100
```

In alternativa, è possibile esaminare il [repository di Modelli di avvio rapido di Azure in GitHub](https://github.com/Azure/azure-quickstart-templates) e consultare le cartelle contenenti `vmss` per vedere esempi preesistenti di modelli di distribuzione di set di scalabilità. Per individuare quali modelli usano già i dischi gestiti, è possibile fare riferimento a [questo elenco](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md).

## <a name="api-versions"></a>Versioni dell'API

La versione corrente delle API generalmente disponibili per i set di scalabilità con dischi gestiti è `2016-04-30-preview`. I set di scalabilità con dischi non gestiti continueranno a funzionare come sempre, anche nelle nuove versioni API che supportano il disco gestito. Tuttavia, i set di scalabilità con dischi non gestiti non godranno dei vantaggi dei dischi gestiti, anche in queste nuove versioni API.

## <a name="next-steps"></a>Passaggi successivi

Per altre informazioni sui dischi gestiti in generale, consultare [questo articolo](../storage/storage-managed-disks-overview.md).

Per informazioni su come convertire un modello di Resource Manager per eseguire il provisioning dei set di scalabilità con i dischi gestiti, consultare [questo articolo](./virtual-machine-scale-sets-convert-template-to-md.md). Le stesse modifiche applicate ai modelli di Resource Manager si applicano anche all'API REST di Azure.

Per altre informazioni sull'uso dei dischi di dati gestiti con i set di scalabilità, vedere [questo articolo](./virtual-machine-scale-sets-attached-disks.md).

Per iniziare a usare i set di scalabilità di grandi dimensioni, fare riferimento a [questo articolo](./virtual-machine-scale-sets-placement-groups.md).



