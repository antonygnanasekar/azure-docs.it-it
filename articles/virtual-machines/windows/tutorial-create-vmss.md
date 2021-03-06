---
title: "Creare un set di scalabilità di macchine virtuali Windows in Azure | Documentazione Microsoft"
description: "Creare e distribuire un&quot;applicazione a disponibilità elevata in VM Windows usando un set di scalabilità di macchine virtuali"
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
ms.devlang: 
ms.topic: article
ms.date: 05/01/2017
ms.author: iainfou
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: bbd4f044d85f2e22f27edc44b91fd42aef304ed2
ms.contentlocale: it-it
ms.lasthandoff: 05/03/2017

---

# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-windows"></a>Creare un set di scalabilità di macchine virtuali e distribuire un'app a disponibilità elevata in Windows
Questa esercitazione spiega come un set di scalabilità di macchine virtuali in Azure consenta di adattare rapidamente il numero di macchine virtuali (VM) che eseguono l'app. Un set di scalabilità di macchine virtuali consente di distribuire e gestire un set di macchine virtuali identiche con scalabilità automatica. È possibile adattare manualmente il numero di VM nel set di scalabilità o definire regole di scalabilità automatica in base all'utilizzo della CPU, alla richiesta di memoria o al traffico di rete. Per verificare il funzionamento di un set di scalabilità di macchine virtuali, si compila un sito IIS di base che si esegue tra più VM Windows.

La procedura descritta in questa esercitazione può essere completata usando il modulo [Azure PowerShell](/powershell/azureps-cmdlets-docs/) più recente.


## <a name="scale-set-overview"></a>Informazioni generali sui set di scalabilità
I set di scalabilità usano concetti simili a quelli descritti nell'esercitazione precedente [Creare VM a disponibilità elevata](tutorial-availability-sets.md). Le VM di un set di scalabilità sono distribuite in domini di errore e di aggiornamento esattamente come le VM di un set di disponibilità.

Le VM vengono create in base alle esigenze in un set di scalabilità. È possibile definire regole di scalabilità automatica per controllare le modalità e i tempi di aggiunta e rimozione delle VM dal set di scalabilità. Queste regole possono essere attivate in base a determinate metriche, ad esempio il carico della CPU, l'utilizzo della memoria o il traffico di rete.

I set di scalabilità supportano fino a 1.000 VM quando si usa un'immagine della piattaforma Azure. Per i carichi di lavoro con requisiti significativi di installazione o personalizzazione di VM, si consiglia di [creare un'immagine di VM personalizzata](tutorial-custom-images.md). È possibile creare fino a 100 VM in un set di scalabilità quando si usa un'immagine personalizzata.


## <a name="create-an-app-to-scale"></a>Creare un'app per la scalabilità
Per poter creare un set di scalabilità è prima necessario creare un gruppo di risorse con il comando [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Nell'esempio seguente viene creato un gruppo di risorse denominato *myResourceGroupAutomate* nella posizione *westus*:

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupScaleSet -Location westus
```

In un'esercitazione precedente si è appreso come [automatizzare la configurazione della VM](tutorial-automate-vm-deployment.md) usando l'estensione dello script personalizzata. Creare una configurazione del set di scalabilità e quindi applicare un'estensione dello script personalizzata per installare e configurare IIS:

```powershell
# Create a config object
$vmssConfig = New-AzureRmVmssConfig `
    -Location WestUS `
    -SkuCapacity 2 `
    -SkuName Standard_DS2 `
    -UpgradePolicyMode Automatic

# Define the script for your Custom Script Extension to run
$publicSettings = @{
    "fileUris" = (,"https://raw.githubusercontent.com/iainfoulds/azure-samples/master/automate-iis.ps1");
    "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1"
}

# Use Custom Script Extension to install IIS and configure basic website
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $publicSettings
```

## <a name="create-scale-load-balancer"></a>Creare un bilanciamento del carico di scalabilità
Azure Load Balancer è un bilanciamento del carico di livello 4 (TCP, UDP) che offre disponibilità elevata mediante la distribuzione del traffico in ingresso fra le VM integre. Un probe di integrità del bilanciamento del carico monitora una determinata porta in ogni VM e distribuisce il traffico solo a una VM operativa. Per altre informazioni, vedere l'esercitazione successiva in [Come bilanciare il carico delle macchine virtuali di Windows](tutorial-load-balancer.md).

Creare un bilanciamento del carico con un indirizzo IP pubblico che distribuisce il traffico Web sulla porta 80:

```powershell
# Create a public IP address
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupScaleSet `
  -Location westus `
  -AllocationMethod Static `
  -Name myPublicIP

# Create a frontend and backend IP pool
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name myFrontEndPool `
  -PublicIpAddress $publicIP
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool

# Create the load balancer
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroupScaleSet `
  -Name myLoadBalancer `
  -Location westus `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool

# Create a load balancer health probe on port 80
Add-AzureRmLoadBalancerProbeConfig -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2

# Create a load balancer rule to distribute traffic on port 80
Add-AzureRmLoadBalancerRuleConfig `
  -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80

# Update the load balancer configuration
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="create-a-scale-set"></a>Creare un set di scalabilità
Ora si può creare un set di scalabilità di macchine virtuali con [New-AzureRmVmss](/powershell/module/azurerm.compute/new-azurermvm). Nell'esempio seguente viene creato un set di scalabilità denominato *myScaleSet*:

```powershell
# Reference a virtual machine image from the gallery
Set-AzureRmVmssStorageProfile $vmssConfig `
  -ImageReferencePublisher MicrosoftWindowsServer `
  -ImageReferenceOffer WindowsServer `
  -ImageReferenceSku 2016-Datacenter `
  -ImageReferenceVersion latest

# Set up information for authenticating with the virtual machine
Set-AzureRmVmssOsProfile $vmssConfig `
  -AdminUsername azureuser `
  -AdminPassword P@ssword! `
  -ComputerNamePrefix myVM

# Create the virtual network resources
$subnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name "mySubnet" `
  -AddressPrefix 10.0.0.0/24
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName "myResourceGroupScaleSet" `
  -Name "myVnet" `
  -Location "westus" `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $subnet
$ipConfig = New-AzureRmVmssIpConfig `
  -Name "myIPConfig" `
  -LoadBalancerBackendAddressPoolsId $lb.BackendAddressPools[0].Id `
  -SubnetId $vnet.Subnets[0].Id

# Attach the virtual network to the config object
Add-AzureRmVmssNetworkInterfaceConfiguration `
  -VirtualMachineScaleSet $vmssConfig `
  -Name "network-config" `
  -Primary $true `
  -IPConfiguration $ipConfig

# Create the scale set with the config object (this step might take a few minutes)
New-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -Name myScaleSet `
  -VirtualMachineScaleSet $vmssConfig     
```

La creazione e la configurazione di tutte le macchine virtuali e risorse del set di scalabilità richiedono alcuni minuti.


## <a name="test-your-app"></a>Test dell'app
Per vedere il sito Web IIS in azione, ottenere l'indirizzo IP pubblico del servizio di bilanciamento del carico con il comando [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). L'esempio seguente ottiene l'indirizzo IP per *myPublicIP* creato come parte del set di scalabilità:

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupScaleSet -Name myPublicIP | select IpAddress
```

Immettere l'indirizzo IP pubblico in un Web browser. Verrà visualizzata l'app, con il nome host della VM a cui il servizio di bilanciamento del carico ha distribuito il traffico:

![Esecuzione del sito IIS](./media/tutorial-create-vmss/running-iis-site.png)

Per verificare il funzionamento del set di scalabilità, è possibile imporre l'aggiornamento del Web browser per visualizzare la distribuzione del traffico da parte del bilanciamento del carico tra tutte le VM che eseguono l'app.


## <a name="management-tasks"></a>Attività di gestione
Nel ciclo di vita del set di scalabilità, potrebbe essere necessario eseguire una o più attività di gestione. Si potrebbe anche voler creare script per automatizzare le attività di ciclo di vita. Azure PowerShell offre un modo rapido per eseguire queste operazioni. Di seguito vengono illustrate alcune attività comuni.

### <a name="view-vms-in-a-scale-set"></a>Visualizzare le VM in un set di scalabilità
Per visualizzare l'elenco delle VM in esecuzione nel set di scalabilità, usare [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) come indicato di seguito:

```powershell
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -VMScaleSetName myScaleSet

# Loop through the instanaces in your scale set
for ($i=0; $i -le ($set.Sku.Capacity - 1); $i++) {
    Get-AzureRmVmssVM -ResourceGroupName myResourceGroupScaleSet `
      -VMScaleSetName myScaleSet `
      -InstanceId $i
}
```


### <a name="increase-or-decrease-vm-instances"></a>Aumentare o diminuire le istanze delle macchine virtuali
Per visualizzare il numero di istanze attualmente presenti in un set di scalabilità, usare il comando [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) ed eseguire una query su *sku.capacity*:

```powershell
Get-AzureRmVmss -ResourceGroupName myResourceGroupScaleSet `
    -VMScaleSetName myScaleSet | `
    Select -ExpandProperty Sku
```

È possibile quindi aumentare o ridurre manualmente il numero di macchine virtuali nel set di scalabilità con [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss). L'esempio seguente imposta il numero di VM del set di scalabilità su *5*:

```powershell
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -VMScaleSetName myScaleSet

# Set and update the capacity of your scale set
$scaleset.sku.capacity = 5
Update-AzureRmVmss -ResourceGroupName myResourceGroupScaleSet `
    -Name myScaleSet `
    -VirtualMachineScaleSet $scaleset
```

Sono necessari alcuni minuti per aggiornare il numero specificato di istanze del set di scalabilità.


### <a name="configure-autoscale-rules"></a>Configurare le regole di scalabilità automatica
Anziché scalare manualmente il numero di istanze del set di scalabilità, si definiscono regole di scalabilità automatica. Queste regole monitorano le istanze nel set di scalabilità e rispondono di conseguenza in base alle metriche e alle soglie definite. L'esempio seguente scala orizzontalmente il numero di istanze di uno quando il carico della CPU medio è maggiore del 60% per un periodo di 5 minuti. Se il carico della CPU medio scende poi sotto il 30% per un periodo di 5 minuti, le istanze vengono ridotte di una:

```powershell
# Define your scale set information
$mySubscriptionId = (Get-AzureRmSubscription).SubscriptionId
$myResourceGroup = "myResourceGroupScaleSet"
$myScaleSet = "myScaleSet"
$myLocation = "West US"

# Create a scale up rule to increase the number instances after 60% average CPU usage exceeded for a 5 minute period
$myRuleScaleUp = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -Operator GreaterThan `
  -MetricStatistic Average `
  -Threshold 60 `
  -TimeGrain 00:01:00 `
  -TimeWindow 00:05:00 `
  -ScaleActionCooldown 00:05:00 `
  -ScaleActionDirection Increase `
  -ScaleActionValue 1

# Create a scale down rule to decrease the number of instances after 30% average CPU usage over a 5 minute period
$myRuleScaleDown = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -Operator LessThan `
  -MetricStatistic Average `
  -Threshold 30 `
  -TimeGrain 00:01:00 `
  -TimeWindow 00:05:00 `
  -ScaleActionCooldown 00:05:00 `
  -ScaleActionDirection Decrease `
  -ScaleActionValue 1

# Create a scale profile with your scale up and scale down rules
$myScaleProfile = New-AzureRmAutoscaleProfile `
  -DefaultCapacity 2  `
  -MaximumCapacity 10 `
  -MinimumCapacity 2 `
  -Rules $myRuleScaleUp,$myRuleScaleDown `
  -Name "autoprofile"

# Apply the autoscale rules
Add-AzureRmAutoscaleSetting `
  -Location $myLocation `
  -Name "autosetting" `
  -ResourceGroup $myResourceGroup `
  -TargetResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -AutoscaleProfiles $myScaleProfile
```


## <a name="next-steps"></a>Passaggi successivi
In questa esercitazione è stato descritto come creare un set di scalabilità di macchine virtuali. Passare all'esercitazione successiva per maggiori informazioni sui concetti di bilanciamento del carico per le macchine virtuali.

[Bilanciare il carico delle macchine virtuali](tutorial-load-balancer.md)
