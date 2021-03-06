---
title: Guida introduttiva per i cluster Kubernetes in Azure | Documentazione Microsoft
description: Distribuire e iniziare a usare un cluster Kubernetes nel servizio contenitore di Azure
services: container-service
documentationcenter: 
author: anhowe
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 8da267e8-2aeb-4c24-9a7a-65bdca3a82d6
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/05/2017
ms.author: anhowe
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: 5c529ae41b42d276d37e6103305e33ed04694e18
ms.lasthandoff: 04/07/2017

---

# <a name="get-started-with-a-kubernetes-cluster-in-container-service"></a>Introduzione a un cluster Kubernetes nel servizio contenitore


Questa procedura dettagliata illustra come usare i comandi dell'interfaccia della riga di comando di Azure 2.0 per creare un cluster Kubernetes nel servizio contenitore di Azure. È quindi possibile usare lo strumento `kubectl` da riga di comando per iniziare a lavorare con i contenitori nel cluster.

L'immagine seguente illustra l'architettura di un cluster del servizio contenitore con un master e due agenti. Il master gestisce l'API REST Kubernetes. I nodi agente vengono raggruppati in un set di disponibilità di Azure ed eseguono i contenitori. Tutte le macchine virtuali si trovano nella stessa rete privata virtuale e sono completamente accessibili le une alle altre.

![Immagine del cluster Kubernetes in Azure](media/container-service-kubernetes-walkthrough/kubernetes.png)

## <a name="prerequisites"></a>Prerequisiti
Questa procedura dettagliata presuppone che sia installata e configurata [l'interfaccia della riga di comando di Azure 2.0](/cli/azure/install-az-cli2). 

Gli esempi di comandi presuppongono che si esegua l'interfaccia della riga di comando di Azure in una shell Bash, comune in Linux e macOS. Se si esegue l'interfaccia della riga di comando di Azure su un client Windows, alcuni script e file di sintassi possono variare a seconda della shell dei comandi. 

## <a name="create-your-kubernetes-cluster"></a>Creare il cluster Kubernetes

Di seguito vengono elencati brevi comandi di shell che usano l'interfaccia della riga di comando di Azure 2.0 per creare il cluster. 

### <a name="create-a-resource-group"></a>Creare un gruppo di risorse
Per creare il cluster, prima di tutto è necessario creare un gruppo di risorse in una posizione specifica. Eseguire comandi simili ai seguenti:

```azurecli
RESOURCE_GROUP=my-resource-group
LOCATION=westus
az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="create-a-cluster"></a>Creare un cluster
Dopo avere creato il gruppo di risorse, è possibile creare un cluster in tale gruppo. Nell'esempio seguente viene usata l'opzione `--generate-ssh-keys`, che genera i file delle chiavi private e pubbliche SSH necessari per la distribuzione qualora non esistessero già nella directory predefinita `~/.ssh/`. 

Inoltre, questo comando genera automaticamente l'[entità servizio di Azure Active Directory](container-service-kubernetes-service-principal.md) usata da un cluster Kubernetes in Azure.

```azurecli
DNS_PREFIX=some-unique-value
CLUSTER_NAME=any-acs-cluster-name
az acs create --orchestrator-type=kubernetes --resource-group $RESOURCE_GROUP --name=$CLUSTER_NAME --dns-prefix=$DNS_PREFIX --generate-ssh-keys
```


Dopo alcuni minuti, il comando viene completato e l'utente deve disporre di un cluster Kubernetes funzionante.

### <a name="connect-to-the-cluster"></a>Connettersi al cluster

Per connettersi al cluster Kubernetes dal computer client si usa [`kubectl`](https://kubernetes.io/docs/user-guide/kubectl/), il client della riga di comando di Kubernetes. 

Se non è già stato installato `kubectl`, è possibile installarlo come segue:

```azurecli
sudo az acs kubernetes install-cli
```
> [!TIP]
> Per impostazione predefinita, questo comando installa il file binario `kubectl` in `/usr/local/bin/kubectl` su un sistema Linux o macOS o in `C:\Program Files (x86)\kubectl.exe` su Windows. Per specificare un percorso di installazione diverso, usare il parametro `--install-location`.
>

Dopo aver installato `kubectl`, assicurarsi che la relativa directory sia nel percorso di sistema oppure aggiungerla al percorso. 


Quindi eseguire il comando seguente per scaricare la configurazione del cluster Kubernetes del master nel file `~/.kube/config`:

```azurecli
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

Per altre opzioni di installazione e per la configurazione di `kubectl`, vedere [Connettersi a un cluster del servizio contenitore di Azure](container-service-connect.md).

A questo punto, sarà possibile accedere al cluster dal computer. Provare a eseguire:

```bash
kubectl get nodes
```

Verificare che i computer siano visualizzati nel cluster.

## <a name="create-your-first-kubernetes-service"></a>Creare il primo servizio Kubernetes

La procedura dettagliata illustra come eseguire queste operazioni:
* Distribuire un'applicazione Docker ed esporla a tutti gli utenti
* Usare `kubectl exec` per eseguire comandi in un contenitore 
* Accedere al dashboard Kubernetes

### <a name="start-a-simple-container"></a>Avviare un contenitore semplice
È possibile eseguire un contenitore semplice (in questo caso, il server Web Nginx) usando:

```bash
kubectl run nginx --image nginx
```

Questo comando avvia il contenitore Docker Nginx in un pod in uno dei nodi.

Per visualizzare il contenitore in esecuzione, eseguire:

```bash
kubectl get pods
```

### <a name="expose-the-service-to-the-world"></a>Esporre il servizio
Per esporre il servizio a tutti gli utenti, creare un `Service` Kubernetes di tipo `LoadBalancer`:

```bash
kubectl expose deployments nginx --port=80 --type=LoadBalancer
```

Con questo comando Kubernetes creerà una regola di bilanciamento del carico di Azure con un indirizzo IP pubblico. La propagazione della modifica al servizio di bilanciamento del carico richiede alcuni minuti. Per altre informazioni, vedere [Bilanciare il carico dei contenitori in un cluster Kubernetes nel servizio contenitore di Azure](container-service-kubernetes-load-balancing.md).

Eseguire il comando seguente per modificare il servizio da `pending` alla visualizzazione di un indirizzo IP esterno:

```bash
watch 'kubectl get svc'
```

  ![Immagine della verifica della transizione da in sospeso a indirizzo IP esterno](media/container-service-kubernetes-walkthrough/kubernetes-nginx3.png)

Dopo la visualizzazione dell'indirizzo IP esterno, è possibile passare all'indirizzo IP nel browser:

  ![Immagine del passaggio a Nginx](media/container-service-kubernetes-walkthrough/kubernetes-nginx4.png)  


### <a name="browse-the-kubernetes-ui"></a>Esplorare l'interfaccia utente Kubernetes
Per visualizzare l'interfaccia Web Kubernetes, è possibile usare:

```bash
kubectl proxy
```
Questo comando esegue in localhost un proxy autenticato semplice, che è possibile usare per visualizzare l'interfaccia utente Web di Kubernetes in esecuzione in [http://localhost:8001/ui](http://localhost:8001/ui). Per altre informazioni, vedere [Uso dell'interfaccia utente Web Kubernetes con il servizio contenitore di Azure](container-service-kubernetes-ui.md).

![Immagine del dashboard Kubernetes](media/container-service-kubernetes-walkthrough/kubernetes-dashboard.png)

### <a name="remote-sessions-inside-your-containers"></a>Sessioni remote nei contenitori
Kubernetes consente di eseguire comandi in un contenitore Docker remoto in esecuzione nel cluster.

```bash
# Get the name of your nginx pods
kubectl get pods
```

Usando il nome del pod, è possibile eseguire un comando remoto sul pod.  Ad esempio:

```bash
kubectl exec <pod name> date
```

È anche possibile ottenere una sessione completamente interattiva usando i flag `-it`:

```bash
kubectl exec <pod name> -it bash
```

![Sessione remota in un contenitore](media/container-service-kubernetes-walkthrough/kubernetes-remote.png)



## <a name="next-steps"></a>Passaggi successivi

Per altre operazioni con il cluster Kubernetes, vedere le risorse seguenti:

* [Kubernetes Bootcamp](https://katacoda.com/embed/kubernetes-bootcamp/1/): illustra come distribuire, ridimensionare ed eseguire il debug di applicazione inserite in contenitori.
* [Guida dell'utente di Kubernetes](http://kubernetes.io/docs/user-guide/): offre informazioni sull'esecuzione di programmi in un cluster Kubernetes esistente.
* [Esempi di Kubernetes](https://github.com/kubernetes/kubernetes/tree/master/examples): fornisce esempi relativi all'esecuzione di applicazioni effettive con Kubernetes.

