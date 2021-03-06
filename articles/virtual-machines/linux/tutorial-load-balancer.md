---
title: Come bilanciare il carico delle macchine virtuali di Linux in Azure | Microsoft Docs
description: "Informazioni su come usare Azure Load Balancer per creare un&quot;applicazione sicura e a disponibilità elevata in tre macchine virtuali di Linux"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/17/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: abdbb9a43f6f01303844677d900d11d984150df0
ms.openlocfilehash: 105ff3614a05926d2bc2f236837bb4d5d95fcf32
ms.lasthandoff: 04/21/2017

---

# <a name="how-to-load-balance-linux-virtual-machines-in-azure-to-create-a-highly-available-application"></a>Come bilanciare il carico per le macchine virtuali di Linux in Azure per creare un'applicazione a disponibilità elevata
In questa esercitazione vengono illustrati i diversi componenti di Azure Load Balancer che distribuiscono il traffico e garantiscono una disponibilità elevata. Per visualizzare il bilanciamento del carico in azione, è necessario compilare un app Node.js in esecuzione su tre macchine virtuali, VM, di Linux.

I passaggi descritti in questa esercitazione possono essere eseguiti usando la versione più recente dell'[interfaccia della riga di comando di Azure 2.0](/cli/azure/install-azure-cli).


## <a name="azure-load-balancer-overview"></a>Panoramica di Azure Load Balancer
Azure Load Balancer è un bilanciamento del carico di livello 4 (TCP, UDP) che offre disponibilità elevata mediante la distribuzione del traffico in ingresso nelle macchine virtuali integre. Un probe di integrità del bilanciamento del carico monitora una determinata porta in ogni VM e distribuisce il traffico solo a una VM operativa.

Definire una configurazione IP front-end che contenga uno o più indirizzi IP pubblici. Questa configurazione IP front-end garantisce l'accessibilità al bilanciamento del carico e alle applicazioni tramite Internet. 

Le macchine virtuali si connettono a un bilanciamento del carico tramite la scheda di interfaccia di rete virtuale (NIC). Per distribuire il traffico alle macchine virtuali, è necessario che un pool di indirizzi back-end contenga gli indirizzi IP delle schede di interfaccia di rete virtuale connesse al bilanciamento del carico.

Per controllare il flusso del traffico, è necessario definire le regole di bilanciamento del carico per porte e protocolli specifici che eseguono il mapping delle macchine virtuali.

Se è stata eseguita l'esercitazione precedente per [creare un set di scalabilità della macchina virtuale](tutorial-create-vmss.md), è stato creato un servizio di bilanciamento del carico. Tutti questi componenti sono stati configurati come parte del set di scalabilità.


## <a name="create-azure-load-balancer"></a>Creare un Azure Load Balancer
Questa sezione descrive dettagliatamente come creare e configurare ogni componente del bilanciamento del carico. Per poter creare un servizio di bilanciamento del carico, è prima necessario creare un gruppo di risorse con [az group create](/cli/azure/group#create). Nell'esempio seguente viene creato un gruppo di risorse denominato `myRGLoadBalancer` nella località `westus`:

```azurecli
az group create --name myRGLoadBalancer --location westus
```

### <a name="create-a-public-ip-address"></a>Creare un indirizzo IP pubblico
Per accedere all'app in Internet, assegnare un indirizzo IP pubblico al servizio di bilanciamento del carico. Creare un indirizzo IP pubblico con [az network public-ip create](/cli/azure/public-ip#create). L'esempio seguente crea un indirizzo IP pubblico denominato `myPublicIP` nel gruppo di risorse `myRGLoadBalancer`:

```azurecli
az network public-ip create --resource-group myRGLoadBalancer --name myPublicIP
```

### <a name="create-a-load-balancer"></a>Creare un servizio di bilanciamento del carico
Creare un servizio di bilanciamento del carico con [az network lb create](/cli/azure/network/lb#create). Nell'esempio seguente viene creato un servizio di bilanciamento del carico denominato `myLoadBalancer` e viene assegnato l'indirizzo `myPublicIP` alla configurazione IP front-end:

```azurecli
az network lb create \
    --resource-group myRGLoadBalancer \
    --name myLoadBalancer \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --public-ip-address myPublicIP
```

### <a name="create-a-health-probe"></a>Creare un probe di integrità
Per consentire al servizio di bilanciamento del carico di monitorare lo stato dell'app, si usa un probe di integrità. Il probe di integrità aggiunge o rimuove in modo dinamico le VM nella rotazione del servizio di bilanciamento del carico in base alla rispettiva risposta ai controlli di integrità. Per impostazione predefinita, una VM viene rimossa dalla distribuzione del servizio di bilanciamento del carico dopo due errori consecutivi a intervalli di 15 secondi. Un probe di integrità viene creato in base a un protocollo o una specifica pagina di controllo integrità per l'app. 

Nell'esempio seguente viene creato un probe TCP. È anche possibile creare probe HTTP personalizzati per i controlli di integrità con granularità fine. Quando si usa un probe HTTP personalizzato, è necessario creare la pagina di controllo di integrità, ad esempio `healthcheck.js`. Il probe deve restituire la risposta **HTTP 200 OK** affinché il servizio di bilanciamento del carico mantenga l'host nella rotazione.

Per creare un probe di integrità TCP, usare con [az network lb probe create](/cli/azure/network/lb/probe#create). Nell'esempio seguente viene creato un probe di integrità denominato `myHealthProbe`:

```azurecli
az network lb probe create \
    --resource-group myRGLoadBalancer \
    --lb-name myLoadBalancer \
    --name myHealthProbe \
    --protocol tcp \
    --port 80
```

### <a name="create-a-load-balancer-rule"></a>Creare una regola di bilanciamento del carico
Una regola di bilanciamento del carico consente di definire come il traffico verrà distribuito alle VM. Definire la configurazione IP front-end per il traffico in ingresso e il pool IP di back-end affinché riceva il traffico, insieme alla porta di origine e di destinazione necessaria. Per assicurarsi che solo le macchine virtuali integre ricevano il traffico, è necessario anche definire il probe di integrità da usare.

Creare una regola di bilanciamento del carico con [az network lb rule create](/cli/azure/network/lb/rule#create). L'esempio seguente crea una regola denominata `myLoadBalancerRule`, usa il probe di integrità `myHealthProbe` e bilancia il traffico sulla porta `80`:

```azurecli
az network lb rule create \
    --resource-group myRGLoadBalancer \
    --lb-name myLoadBalancer \
    --name myLoadBalancerRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe
```


## <a name="configure-virtual-network"></a>Configurare la rete virtuale
Prima di distribuire alcune macchine virtuali e testare il servizio di bilanciamento, creare le risorse di rete virtuale di supporto. Per altre informazioni sulle reti virtuali, vedere l'esercitazione [Gestire le reti virtuali di Azure](tutorial-virtual-network.md).

### <a name="create-network-resources"></a>Creare risorse di rete
Creare una rete virtuale con [az network vnet create](/cli/azure/vnet#create). L'esempio seguente creata una rete virtuale denominata `myVnet` con una subnet denominata `mySubnet`:

```azurecli
az network vnet create --resource-group myRGLoadBalancer --name myVnet --subnet-name mySubnet
```

Per aggiungere un gruppo di sicurezza di rete usare [az network nsg create](/cli/azure/network/nsg#create). L'esempio seguente crea un gruppo di sicurezza di rete denominato `myNetworkSecurityGroup`:

```azurecli
az network nsg create --resource-group myRGLoadBalancer --name myNetworkSecurityGroup
```

Creare una regola del gruppo di sicurezza di rete con [az network nsg rule create](/cli/azure/network/nsg/rule#create). L'esempio seguente crea una regola del gruppo di sicurezza di rete denominata `myNetworkSecurityGroupRule`:

```azurecli
az network nsg rule create \
    --resource-group myRGLoadBalancer \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --priority 1001 \
    --protocol tcp \
    --destination-port-range 80
```

Le schede di interfaccia di rete virtuale vengono create con [az network nic create](/cli/azure/network/nic#create). L'esempio seguente crea tre schede di interfaccia di rete virtuali, Una scheda di interfaccia di rete virtuale per ogni VM creata per l'app nei passaggi successivi. È possibile creare altre schede di interfaccia di rete virtuale e macchine virtuali in qualsiasi momento e aggiungerle al bilanciamento del carico:

```bash
for i in `seq 1 3`; do
    az network nic create \
        --resource-group myRGLoadBalancer \
        --name myNic$i \
        --vnet-name myVnet \
        --subnet mySubnet \
        --network-security-group myNetworkSecurityGroup \
        --lb-name myLoadBalancer \
        --lb-address-pools myBackEndPool
done
```

## <a name="create-virtual-machines"></a>Creare macchine virtuali

### <a name="create-cloud-init-config"></a>Creare una configurazione cloud-init
In un'esercitazione precedente, [How to customize a Linux virtual machine on first boot](tutorial-automate-vm-deployment.md) (Come personalizzare una macchina virtuale Linux al primo avvio), è stato descritto come personalizzare una macchina virtuale al primo avvio con cloud-init. È possibile usare lo stesso file di configurazione cloud-init per installare NGINX ed eseguire una semplice app Node.js "Hello World". Creare un file denominato `cloud-init.txt` e incollare la configurazione seguente:

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

### <a name="create-virtual-machines"></a>Creare macchine virtuali
Per aumentare la disponibilità elevata dell'app, posizionare le macchine virtuali in un set di disponibilità. Per altre informazioni sui set di disponibilità, vedere l'esercitazione precedente [Come creare macchine virtuali a disponibilità elevata](tutorial-availability-sets.md).

Creare un set di disponibilità con [az vm availability-set create](/cli/azure/vm/availability-set#create). Nell'esempio seguente viene creato un set di disponibilità chiamato `myAvailabilitySet`:

```azurecli
az vm availability-set create \
    --resource-group myRGLoadBalancer \
    --name myAvailabilitySet \
    --platform-fault-domain-count 3 \
    --platform-update-domain-count 2
```

Ora è possibile creare le VM con [az vm create](/cli/azure/vm#create). L'esempio seguente crea tre VM e genera le chiavi SSH, se non sono già presenti:

```bash
for i in `seq 1 3`; do
    az vm create \
        --resource-group myRGLoadBalancer \
        --name myVM$i \
        --availability-set myAvailabilitySet \
        --nics myNic$i \
        --image Canonical:UbuntuServer:14.04.4-LTS:latest \
        --admin-username azureuser \
        --generate-ssh-keys \
        --custom-data cloud-init.txt \
        --no-wait
done
```

La creazione e la configurazione di tutte e tre le VM richiedono alcuni minuti. Il probe di integrità del servizio di bilanciamento del carico rileva quando l'app è in esecuzione in ogni VM. Quando l'app è in esecuzione, la regola di bilanciamento del carico inizia a distribuire il traffico.


## <a name="test-load-balancer"></a>Testare il bilanciamento del carico
Ottenere l'indirizzo IP pubblico del servizio di bilanciamento del carico con [az network public-ip show](/cli/azure/network/public-ip#show). L'esempio seguente ottiene l'indirizzo IP `myPublicIP` creato in precedenza:

```azurecli
az network public-ip show \
    --resource-group myRGLoadBalancer \
    --name myPublicIP \
    --query [ipAddress] \
    --output tsv
```

Sarà quindi possibile immettere l'indirizzo IP pubblico in un Web browser. Verrà visualizzata l'app, con il nome host della macchina virtuale a cui il servizio di bilanciamento del carico ha distribuito il traffico, come nell'esempio seguente:

![Esecuzione dell'app Node.js](./media/tutorial-load-balancer/running-nodejs-app.png)

Per verificare la distribuzione del traffico tra tutte e tre le VM che eseguono l'app da parte del servizio di bilanciamento del carico, forzare l'aggiornamento del Web browser.


## <a name="add-and-remove-vms"></a>aggiunta e rimozione di VM
Potrebbe essere necessario eseguire attività di manutenzione sulle VM che eseguono l'app, ad esempio per installare aggiornamenti del sistema operativo, oppure aggiungere altre VM per gestire un aumento del traffico verso l'app. Questa sezione illustra come rimuovere o aggiungere una VM nel servizio di bilanciamento del carico.

### <a name="remove-a-vm-from-the-load-balancer"></a>Rimuovere una VM dal servizio di bilanciamento del carico
È possibile rimuovere una VM dal pool di indirizzi back-end con [az network nic ip-config address-pool remove](/cli/azure/network/nic/ip-config/address-pool#remove). L'esempio seguente rimuove la scheda di interfaccia di rete virtuale per **myVM2** da `myLoadBalancer`:

```azurecli
az network nic ip-config address-pool remove \
    --resource-group myRGLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool 
```

Per verificare la distribuzione del traffico nelle due VM restanti che eseguono l'app da parte del servizio di bilanciamento del carico, forzare l'aggiornamento del Web browser. È ora possibile eseguire attività di manutenzione sulla VM, ad esempio installare aggiornamenti del sistema operativo o eseguire un riavvio della VM.

### <a name="add-a-vm-to-the-load-balancer"></a>Aggiungere una VM al servizio di bilanciamento del carico
Al termine della manutenzione di una VM o se è necessario espandere la capacità, è possibile aggiungere una VM al pool di indirizzi back-end con [az network nic ip-config address-pool add](/cli/azure/network/nic/ip-config/address-pool#add). L'esempio seguente aggiunge la scheda di interfaccia di rete virtuale per **myVM2** a `myLoadBalancer`:

```azurecli
az network nic ip-config address-pool add \
    --resource-group myRGLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool
```


## <a name="next-steps"></a>Passaggi successivi
In questa esercitazione è stato illustrato come creare un servizio di bilanciamento del carico per le macchine virtuali. Passare all'esercitazione successiva per altre informazioni sui componenti della rete virtuale di Azure.

[Gestire la rete VM](tutorial-virtual-network.md)

