---
title: 'Automazione di Azure DocumentDB: gestione con Powershell | Documentazione Microsoft'
description: "Usare Azure Powershell per gestire gli account del database DocumentDB. DocumentDB è un database NoSQL basato su cloud per dati JSON."
services: documentdb
author: dmakwana
manager: jhubbard
editor: 
tags: azure-resource-manager
documentationcenter: 
ms.assetid: 
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: dimakwan
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 1d7691fd5248691f42ad31224f2fff9f7f0d4b9e
ms.lasthandoff: 04/03/2017


---
# <a name="automate-azure-documentdb-account-management-using-azure-powershell"></a>Automatizzare la gestione degli account Azure DocumentDB usando Azure Powershell
> [!div class="op_single_selector"]
> * [Portale di Azure](documentdb-create-account.md)
> * [Interfaccia della riga di comando di Azure 1.0](documentdb-automation-resource-manager-cli-nodejs.md)
> * [Interfaccia della riga di comando di Azure 2.0](documentdb-automation-resource-manager-cli.md)
> * [Azure PowerShell](documentdb-manage-account-with-powershell.md)

La guida seguente illustra i comandi per automatizzare la gestione degli account del database DocumentDB usando Azure Powershell. Include anche i comandi per gestire le chiavi dell'account e le priorità di failover in [account di database tra più aree][scaling-globally]. L'aggiornamento dell'account del database consente di modificare i criteri di coerenza e di aggiungere/rimuovere le aree. Per la gestione multipiattaforma dell'account del database DocumentDB, è possibile usare l'[interfaccia della riga di comando di Azure](documentdb-automation-resource-manager-cli.md), l'[API REST del provider di risorse][rp-rest-api] o il [portale di Azure](documentdb-create-account.md).

## <a name="getting-started"></a>Introduzione

Seguire le istruzioni in [Come installare e configurare Azure PowerShell][powershell-install-configure] per installare e accedere all'account Azure Resource Manager in PowerShell.

### <a name="notes"></a>Note

* Per eseguire i comandi seguenti senza richiedere la conferma dell'utente, aggiungere il flag `-Force` al comando.
* Tutti i comandi seguenti sono sincroni.

## <a id="create-documentdb-account-powershell"></a> Creare un account di database DocumentDB

Questo comando consente di creare un account di database DocumentDB. Configurare il nuovo account del database come ad area singola o [a più aree][scaling-globally] con un determinato [criterio di coerenza](documentdb-consistency-levels.md).

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $DocumentDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name>  -Location "<resource-group-location>" -Name <database-account-name> -PropertyObject $DocumentDBProperties
    
* `<write-region-location>` Nome della località dell'area di scrittura dell'account del database. La località è necessaria per impostare il valore della priorità di failover su 0. Deve essere presente esattamente un'area di scrittura per ogni account di database.
* `<read-region-location>` Nome della località dell'area di lettura dell'account del database. La località è necessaria per impostare il valore della priorità di failover come maggiore di 0. Possono essere presenti più aree di lettura per ogni account di database.
* `<ip-range-filter>` Specifica il set di indirizzi IP e di intervalli di indirizzi IP nel formato CIDR da includere come elenco consentito di IP client per un determinato account di database. Gli intervalli di indirizzi IP o i singoli indirizzi IP devono essere delimitati da virgole e non devono contenere spazi. Per altre informazioni, vedere [Supporto del firewall di DocumentDB](documentdb-firewall-support.md).
* `<default-consistency-level>` Livello di coerenza predefinito dell'account DocumentDB. Per altre informazioni, vedere [Livelli di coerenza in DocumentDB](documentdb-consistency-levels.md).
* `<max-interval>` Quando è usato con il livello di coerenza Decadimento ristretto, questo valore rappresenta l'intervallo di tempo di decadimento (in secondi) tollerato. L'intervallo consentito per questo valore è compreso tra 1 e 100.
* `<max-staleness-prefix>` Quando è usato con il livello di coerenza Decadimento ristretto, questo valore rappresenta il numero di richieste non aggiornate tollerate. L'intervallo consentito per questo valore è compreso tra 1 e 2.147.483.647.
* `<resource-group-name>` Nome del [gruppo di risorse di Azure][azure-resource-groups] a cui appartiene il nuovo account del database DocumentDB.
* `<resource-group-location>` Località del gruppo di risorse di Azure a cui appartiene il nuovo account del database DocumentDB.
* `<database-account-name>` Nome dell'account del database DocumentDB da creare. Può contenere solo lettere minuscole, numeri, il carattere "-" e deve essere compreso fra 3 e 50 caratteri.

Esempio: 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $DocumentDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Location "West US" -Name "docdb-test" -PropertyObject $DocumentDBProperties

### <a name="notes"></a>Note
* L'esempio precedente crea un account di database con due aree. È anche possibile creare un account di database con un'area (designata come area di scrittura con un valore di priorità di failover pari a 0) o con più di due aree. Per altre informazioni, vedere gli [account di database tra più aree][scaling-globally].
* Le località devono essere aree in cui DocumentDB è disponibile a livello generale. L'elenco corrente delle aree geografiche è disponibile nella [pagina Aree di Azure](https://azure.microsoft.com/regions/#services).

## <a id="update-documentdb-account-powershell"> </a> Aggiornare un account di database DocumentDB

Questo comando consente di aggiornare le proprietà di un account di database DocumentDB e include il criterio di coerenza e le località in cui esiste l'account di database.

> [!NOTE]
> Il comando consente di aggiungere e rimuovere aree, ma non di modificare le priorità di failover. Per modificare le priorità di failover, vedere [sotto](#modify-failover-priority-powershell).

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $DocumentDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name> -Name <database-account-name> -PropertyObject $DocumentDBProperties
    
* `<write-region-location>` Nome della località dell'area di scrittura dell'account del database. La località è necessaria per impostare il valore della priorità di failover su 0. Deve essere presente esattamente un'area di scrittura per ogni account di database.
* `<read-region-location>` Nome della località dell'area di lettura dell'account del database. La località è necessaria per impostare il valore della priorità di failover come maggiore di 0. Possono essere presenti più aree di lettura per ogni account di database.
* `<default-consistency-level>` Livello di coerenza predefinito dell'account DocumentDB. Per altre informazioni, vedere [Livelli di coerenza in DocumentDB](documentdb-consistency-levels.md).
* `<ip-range-filter>` Specifica il set di indirizzi IP e di intervalli di indirizzi IP nel formato CIDR da includere come elenco consentito di IP client per un determinato account di database. Gli intervalli di indirizzi IP o i singoli indirizzi IP devono essere delimitati da virgole e non devono contenere spazi. Per altre informazioni, vedere [Supporto del firewall di DocumentDB](documentdb-firewall-support.md).
* `<max-interval>` Quando è usato con il livello di coerenza Decadimento ristretto, questo valore rappresenta l'intervallo di tempo di decadimento (in secondi) tollerato. L'intervallo consentito per questo valore è compreso tra 1 e 100.
* `<max-staleness-prefix>` Quando è usato con il livello di coerenza Decadimento ristretto, questo valore rappresenta il numero di richieste non aggiornate tollerate. L'intervallo consentito per questo valore è compreso tra 1 e 2.147.483.647.
* `<resource-group-name>` Nome del [gruppo di risorse di Azure][azure-resource-groups] a cui appartiene il nuovo account del database DocumentDB.
* `<resource-group-location>` Località del gruppo di risorse di Azure a cui appartiene il nuovo account del database DocumentDB.
* `<database-account-name>` Nome dell'account del database DocumentDB da aggiornare.

Esempio: 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $DocumentDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -PropertyObject $DocumentDBProperties

## <a id="delete-documentdb-account-powershell"></a> Eliminare un account di database DocumentDB

Questo comando consente di eliminare un account di database DocumentDB esistente.

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"
    
* `<resource-group-name>` Nome del [gruppo di risorse di Azure][azure-resource-groups] a cui appartiene il nuovo account del database DocumentDB.
* `<database-account-name>` Nome dell'account del database DocumentDB da eliminare.

Esempio:

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="get-documentdb-properties-powershell"></a> Ottenere le proprietà di un account di database DocumentDB

Questo comando consente di ottenere le proprietà di un account di database DocumentDB esistente.

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>` Nome del [gruppo di risorse di Azure][azure-resource-groups] a cui appartiene il nuovo account del database DocumentDB.
* `<database-account-name>` Nome dell'account del database DocumentDB.

Esempio:

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="update-tags-powershell"> </a> Aggiornare i tag di un account di database DocumentDB

L'esempio seguente illustra come impostare i [tag delle risorse di Azure][azure-resource-tags] per l'account del database DocumentDB.

> [!NOTE]
> Questo comando può essere combinato con i comandi di creazione o aggiornamento aggiungendo il flag `-Tags` con il parametro corrispondente.

Esempio:

    $tags = @{"dept" = "Finance”; environment = “Production”}
    Set-AzureRmResource -ResourceType “Microsoft.DocumentDB/databaseAccounts”  -ResourceGroupName "rg-test" -Name "docdb-test" -Tags $tags

## <a id="list-account-keys-powershell"></a> Elencare le chiavi dell'account

Quando si crea un account DocumentDB, il servizio genera due chiavi di accesso principali che possono essere usate per l'autenticazione quando si accede all'account DocumentDB. Fornendo due chiavi di accesso, DocumentDB consente di rigenerare le chiavi senza interruzioni dell'account DocumentDB. Sono disponibili anche chiavi di sola lettura per l'autenticazione delle operazioni di sola lettura. Esistono due chiavi di lettura/scrittura (primaria e secondaria) e due chiavi di sola lettura (primaria e secondaria).

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>` Nome del [gruppo di risorse di Azure][azure-resource-groups] a cui appartiene il nuovo account del database DocumentDB.
* `<database-account-name>` Nome dell'account del database DocumentDB.

Esempio:

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="list-connection-strings-powershell"></a> Elencare le stringhe di connessione

Per gli account di MongoDB, è possibile recuperare la stringa di connessione per connettere l'app MongoDB all'account del database usando il comando seguente.

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>` Nome del [gruppo di risorse di Azure][azure-resource-groups] a cui appartiene il nuovo account del database DocumentDB.
* `<database-account-name>` Nome dell'account del database DocumentDB.

Esempio:

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="regenerate-account-key-powershell"></a> Rigenerare una chiave dell'account

È consigliabile modificare periodicamente le chiavi di accesso all'account DocumentDB per mantenere più sicure le connessioni. Vengono assegnate due chiavi di accesso per consentire di mantenere le connessioni all'account DocumentDB con una delle due chiavi mentre si rigenera l'altra.

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"keyKind"="<key-kind>"}

* `<resource-group-name>` Nome del [gruppo di risorse di Azure][azure-resource-groups] a cui appartiene il nuovo account del database DocumentDB.
* `<database-account-name>` Nome dell'account del database DocumentDB.
* `<key-kind>` Uno dei quattro tipi di chiavi: ["Primary"|"Secondary"|"PrimaryReadonly"|"SecondaryReadonly"] che si vuole rigenerare.

Esempio:

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"keyKind"="Primary"}

## <a id="modify-failover-priority-powershell"></a> Modificare la priorità di failover di un account di database DocumentDB

Per gli account di database tra più aree, è possibile modificare la priorità di failover delle diverse aree in cui esiste l'account del database DocumentDB. Per altre informazioni sul failover nell'account del database DocumentDB, vedere [Distribuire i dati a livello globale con DocumentDB][distribute-data-globally].

    $failoverPolicies = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0},@{"locationName"="<read-region-location>"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"failoverPolicies"=$failoverPolicies}

* `<write-region-location>` Nome della località dell'area di scrittura dell'account del database. La località è necessaria per impostare il valore della priorità di failover su 0. Deve essere presente esattamente un'area di scrittura per ogni account di database.
* `<read-region-location>` Nome della località dell'area di lettura dell'account del database. La località è necessaria per impostare il valore della priorità di failover come maggiore di 0. Possono essere presenti più aree di lettura per ogni account di database.
* `<resource-group-name>` Nome del [gruppo di risorse di Azure][azure-resource-groups] a cui appartiene il nuovo account del database DocumentDB.
* `<database-account-name>` Nome dell'account del database DocumentDB.

Esempio:

    $failoverPolicies = @(@{"locationName"="East US"; "failoverPriority"=0},@{"locationName"="West US"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"failoverPolicies"=$failoverPolicies}

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[powershell-install-configure]: https://docs.microsoft.com/en-us/azure/powershell-install-configure
[scaling-globally]: https://azure.microsoft.com/en-us/documentation/articles/documentdb-distribute-data-globally/#scaling-across-the-planet
[distribute-data-globally]: https://docs.microsoft.com/en-us/azure/documentdb/documentdb-distribute-data-globally
[azure-resource-groups]: https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview#resource-groups
[azure-resource-tags]: https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-using-tags
[rp-rest-api]: https://docs.microsoft.com/en-us/rest/api/documentdbresourceprovider/
