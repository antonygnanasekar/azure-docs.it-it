---
title: "Creare un set di scalabilità di macchine virtuali Linux in Azure | Microsoft Docs"
description: "Creare e distribuire un&quot;applicazione a disponibilità elevata in macchine virtuali Linux usando un set di scalabilità di macchine virtuali"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 04/18/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: e0bfa7620feeb1bad33dd2fe4b32cb237d3ce158
ms.openlocfilehash: 73167924f95c8cea0ac3cb4651cb3571fb24cc01
ms.lasthandoff: 04/21/2017

---

# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-linux"></a>Creare un set di scalabilità di macchine virtuali e distribuire un'app a disponibilità elevata in Linux
In questa esercitazione viene illustrato come un set di scalabilità di macchine virtuali in Azure consenta di adattare rapidamente il numero di macchine virtuali (VM) che eseguono l'app. Un set di scalabilità di macchine virtuali consente di distribuire e gestire un set di macchine virtuali identiche con scalabilità automatica. È possibile adattare manualmente il numero di macchine virtuali nel set di scalabilità o definire regole di scalabilità automatica in base all'utilizzo della CPU, alla richiesta di memoria o al traffico di rete. Per verificare il funzionamento di un set di scalabilità di macchine virtuali, si compila un'app Node.js che viene eseguita tra più macchine virtuali Linux.

I passaggi descritti in questa esercitazione possono essere eseguiti usando la versione più recente dell'[interfaccia della riga di comando di Azure 2.0](/cli/azure/install-azure-cli).


## <a name="scale-set-overview"></a>Informazioni generali sui set di scalabilità
Un set di scalabilità di macchine virtuali consente di distribuire e gestire un set di macchine virtuali identiche con scalabilità automatica. I set di scalabilità usano gli stessi componenti descritti nell'esercitazione precedente [Creare macchine virtuali a disponibilità elevata](tutorial-availability-sets.md). Le macchine virtuali di un set di scalabilità vengono create in un set di disponibilità e distribuite in domini logici di errore e di aggiornamento.

Le macchine virtuali vengono create in base alle esigenze in un set di scalabilità. È possibile definire regole di scalabilità automatica per controllare le modalità e i tempi di aggiunta o rimozione delle macchine virtuali dal set di scalabilità. Queste regole possono essere attivate in base a determinate metriche, ad esempio il carico della CPU, l'utilizzo della memoria o il traffico di rete.

I set di scalabilità supportano fino a 1000 macchine virtuali quando si usa un'immagine della piattaforma Azure. Per i carichi di lavoro di produzione, è opportuno [creare un'immagine di macchina virtuale personalizzata](tutorial-custom-images.md). È possibile creare fino a 100 macchine virtuali in un set di scalabilità quando si usa un'immagine personalizzata.


## <a name="create-an-app-to-scale"></a>Creare un'app per la scalabilità
Per l'uso in ambiente di produzione, è opportuno [creare un'immagine di macchina virtuale personalizzata](tutorial-custom-images.md) che includa l'applicazione installata e configurata. Per questa esercitazione si esegue la personalizzazione delle macchine virtuali al primo avvio per verificare rapidamente il funzionamento di un set di scalabilità.

In un'esercitazione precedente, [How to customize a Linux virtual machine on first boot](tutorial-automate-vm-deployment.md), è stato descritto come personalizzare una macchina virtuale al primo avvio con cloud-init. È possibile usare lo stesso file di configurazione cloud-init per installare NGINX ed eseguire una semplice app Node.js "Hello World". Creare un file denominato `cloud-init.txt` e incollare la configurazione seguente:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```


## <a name="create-a-scale-set"></a>Creare un set di scalabilità
Per poter creare un set di scalabilità, è prima necessario creare un gruppo di risorse con il comando [az group create](/cli/azure/group#create). Nell'esempio seguente viene creato un gruppo di risorse denominato `myResourceGroupScaleSet` nella località `westus`:

```azurecli
az group create --name myResourceGroupScaleSet --location westus
```

Si può ora creare un set di scalabilità di macchine virtuali con il comando [az vmss create](/cli/azure/vmss#create). Nell'esempio seguente viene creato un set di scalabilità denominato `myScaleSet`, viene usato il file cloud-int per personalizzare la macchina virtuale e vengono generate le chiavi SSH, se non sono presenti:

```azurecli
az vmss create \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --image Canonical:UbuntuServer:14.04.4-LTS:latest \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.txt \
  --admin-username azureuser \
  --generate-ssh-keys      
```

La creazione e la configurazione di tutte le macchine virtuali e risorse del set di scalabilità richiedono alcuni minuti.


## <a name="allow-web-traffic"></a>Consentire il traffico Web
Un bilanciamento del carico è stato creato automaticamente come parte del set di scalabilità di macchine virtuali. Il bilanciamento del carico distribuisce il traffico ad un set di macchine virtuali definite usando le proprie regole. Altre informazioni sui concetti di bilanciamento del carico e sulla configurazione saranno illustrate nella prossima esercitazione, [Come bilanciare il carico di macchine virtuali in Azure](tutorial-load-balancer.md).

Per consentire al traffico di raggiungere l'app Web, creare una regola con il comando [az network lb rule create](/cli/azure/network/lb/rule#create). Nell'esempio seguente viene creata una regola denominata `myLoadBalancerRuleWeb`:

```azurecli
az network lb rule create \
  --resource-group myResourceGroupScaleSet \
  --name myLoadBalancerRuleWeb \
  --lb-name myScaleSetLB \
  --backend-pool-name myScaleSetLBBEPool \
  --backend-port 80 \
  --frontend-ip-name loadBalancerFrontEnd \
  --frontend-port 80 \
  --protocol tcp
```

## <a name="test-your-app"></a>Test dell'app
Per visualizzare l'app Node.js sul Web, ottenere l'indirizzo IP pubblico del bilanciamento del carico con il comando [az network public-ip show](/cli/azure/network/public-ip#show). L'esempio seguente ottiene l'indirizzo IP per `myScaleSetLBPublicIP` creato come parte del set di scalabilità:

```azurecli
az network public-ip show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSetLBPublicIP \
    --query [ipAddress] \
    --output tsv
```

Immettere l'indirizzo IP pubblico in un Web browser. Verrà visualizzata l'app, con il nome host della VM a cui il servizio di bilanciamento del carico ha distribuito il traffico:

![Esecuzione dell'app Node.js](./media/tutorial-create-vmss/running-nodejs-app.png)

Per verificare il funzionamento del set di scalabilità, è possibile imporre l'aggiornamento del Web browser per visualizzare la distribuzione del traffico da parte del bilanciamento del carico tra tutte le macchine virtuali che eseguono l'app.


## <a name="management-tasks"></a>Attività di gestione
Nel ciclo di vita del set di scalabilità, potrebbe essere necessario eseguire una o più attività di gestione. Si potrebbe anche voler creare script per automatizzare le attività di ciclo di vita. L'interfaccia della riga di comando di Azure 2.0 offre un modo rapido per eseguire tali operazioni. Di seguito vengono illustrate alcune attività comuni.

### <a name="view-vms-in-a-scale-set"></a>Visualizzare le macchine virtuali in un set di scalabilità
Per visualizzare un elenco di macchine virtuali in esecuzione nel set di scalabilità, usare il comando [az vmss list-instances](/cli/azure/vmss#list-instances) come indicato di seguito:

```azurecli
az vmss list-instances \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --output table
```

L'output è simile all'esempio seguente:

```azurecli
  InstanceId  LatestModelApplied    Location    Name          ProvisioningState    ResourceGroup            VmId
------------  --------------------  ----------  ------------  -------------------  -----------------------  ------------------------------------
           1  True                  westus      myScaleSet_1  Succeeded            MYRESOURCEGROUPSCALESET  c72ddc34-6c41-4a53-b89e-dd24f27b30ab
           3  True                  westus      myScaleSet_3  Succeeded            MYRESOURCEGROUPSCALESET  44266022-65c3-49c5-92dd-88ffa64f95da
```


### <a name="increase-or-decrease-vm-instances"></a>Aumentare o diminuire le istanze delle macchine virtuali
Per visualizzare il numero di istanze attualmente presenti in un set di scalabilità, usare il comando [az vmss show](/cli/azure/vmss#show) ed eseguire una query su `sku.capacity`:

```azurecli
az vmss show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --query [sku.capacity] \
    --output table
```

È possibile aumentare o ridurre manualmente il numero di macchine virtuali nel set di scalabilità con il comando [az vmss scale](/cli/azure/vmss#scale). L'esempio seguente imposta il numero di macchine virtuali del set di scalabilità a `5`:

```azurecli
az vmss scale \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --new-capacity 5
```

Le regole di scalabilità automatica consentono di definire come aumentare o ridurre il numero di macchine virtuali del set di scalabilità in base alla domanda, ad esempio il traffico di rete o l'utilizzo della CPU. Non è attualmente possibile impostare queste regole nell'interfaccia della riga di comando di Azure 2.0. Usare il [Portale di Azure](https://portal.azure.com) per configurare la scalabilità automatica.

### <a name="get-connection-info"></a>Ottenere informazioni sulla connessione
Per ottenere informazioni sulla connessione delle macchine virtuali nel set di scalabilità, usare [az vmss list-instance-connection-info](/cli/azure/vmss#list-instance-connection-info). Questo comando restituisce l'indirizzo IP pubblico e la porta per ogni macchina virtuale che consente la connessione con SSH:

```azurecli
az vmss list-instance-connection-info --resource-group myResourceGroupScaleSet --name myScaleSet
```


## <a name="next-steps"></a>Passaggi successivi
In questa esercitazione è stato descritto come creare un set di scalabilità di macchine virtuali. Passare all'esercitazione successiva per maggiori informazioni sui concetti di bilanciamento del carico per le macchine virtuali.

[Bilanciare il carico di macchine virtuali](tutorial-load-balancer.md)

