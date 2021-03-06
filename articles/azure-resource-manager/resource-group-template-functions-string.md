---
title: Funzioni del modello di Azure Resource Manager - Stringa | Documentazione Microsoft
description: Informazioni sulle funzioni da usare in un modello di Azure Resource Manager per operare con le stringhe.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/26/2017
ms.author: tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: 54b5b8d0040dc30651a98b3f0d02f5374bf2f873
ms.openlocfilehash: 9b75d0ede3ec1b291936ee0a53778afe10ba91db
ms.contentlocale: it-it
ms.lasthandoff: 04/28/2017


---
# <a name="string-functions-for-azure-resource-manager-templates"></a>Funzioni di stringa nei modelli di Azure Resource Manager

Gestione risorse fornisce le funzioni seguenti per usare le stringhe:

* [base64](#base64)
* [base64ToJson](#base64tojson)
* [base64ToString](#base64tostring)
* [bool](#bool)
* [concat](#concat)
* [contains](#contains)
* [dataUri](#datauri)
* [dataUriToString](#datauritostring)
* [empty](#empty)
* [endsWith](#endswith)
* [first](#first)
* [indexOf](#indexof)
* [last](#last)
* [lastIndexOf](#lastindexof)
* [length](#length)
* [padLeft](#padleft)
* [replace](#replace)
* [skip](#skip)
* [split](#split)
* [startsWith](resource-group-template-functions-string.md#startswith)
* [string](#string)
* [substring](#substring)
* [take](#take)
* [toLower](#tolower)
* [toUpper](#toupper)
* [Trim](#trim)
* [uniqueString](#uniquestring)
* [Uri](#uri)
* [uriComponent](resource-group-template-functions-string.md#uricomponent)
* [uriComponentToString](resource-group-template-functions-string.md#uricomponenttostring)

<a id="base64" />

## <a name="base64"></a>base64
`base64(inputString)`

Restituisce la rappresentazione base64 della stringa di input.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| inputString |Sì |string |Il valore da restituire come rappresentazione base64. |

### <a name="examples"></a>esempi

L'esempio seguente mostra come usare la funzione base64.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

### <a name="return-value"></a>Valore restituito

Stringa contenente la rappresentazione base64.

<a id="base64tojson" />

## <a name="base64tojson"></a>base64ToJson
`base64tojson`

Converte una rappresentazione base64 in un oggetto JSON.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| base64Value |Sì |string |Rappresentazione base64 da convertire in un oggetto JSON. |

### <a name="examples"></a>esempi

L'esempio seguente usa la funzione base64ToJson per convertire un valore base64:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

### <a name="return-value"></a>Valore restituito

Oggetto JSON.

<a id="base64tostring" />

## <a name="base64tostring"></a>base64ToString
`base64ToString(base64Value)`

Converte una rappresentazione base64 in una stringa.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| base64Value |Sì |string |Rappresentazione base64 da convertire in stringa. |

### <a name="examples"></a>esempi

L'esempio seguente usa la funzione base64ToString per convertire un valore base64:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringData": {
            "type": "string",
            "defaultValue": "one, two, three"
        },
        "jsonFormattedData": {
            "type": "string",
            "defaultValue": "{'one': 'a', 'two': 'b'}"
        }
    },
    "variables": {
        "base64String": "[base64(parameters('stringData'))]",
        "base64Object": "[base64(parameters('jsonFormattedData'))]"
    },
    "resources": [
    ],
    "outputs": {
        "base64Output": {
            "type": "string",
            "value": "[variables('base64String')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[base64ToString(variables('base64String'))]"
        },
        "toJsonOutput": {
            "type": "object",
            "value": "[base64ToJson(variables('base64Object'))]"
        }
    }
}
```

### <a name="return-value"></a>Valore restituito

Stringa del valore base64 convertito.

<a id="bool" />

## <a name="bool"></a>bool
`bool(arg1)`

Converte il parametro in un valore booleano.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| arg1 |Sì |Stringa o numero intero |Valore da convertire in un valore booleano. |

### <a name="examples"></a>esempi

L'esempio seguente illustra come usare il parametro bool con un numero intero o una stringa.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "trueString": {
            "value": "[bool('true')]",
            "type" : "bool"
        },
        "falseString": {
            "value": "[bool('false')]",
            "type" : "bool"
        },
        "trueInt": {
            "value": "[bool(1)]",
            "type" : "bool"
        },
        "falseInt": {
            "value": "[bool(0)]",
            "type" : "bool"
        }
    }
}
```

### <a name="return-value"></a>Valore restituito
Valore booleano.

<a id="concat" />

## <a name="concat"></a>concat
`concat (arg1, arg2, arg3, ...)`

Combina più valori stringa e restituisce la stringa concatenata oppure combina più matrici e restituisce la matrice concatenata.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| arg1 |Sì |Stringa o matrice |Il primo valore per la concatenazione. |
| Argomenti aggiuntivi |No |string |Altri valori in ordine sequenziale per la concatenazione. |

### <a name="examples"></a>esempi

L'esempio seguente illustra come combinare due valori stringa e restituisce una stringa concatenata.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "prefix": {
            "type": "string",
            "defaultValue": "prefix"
        }
    },
    "resources": [],
    "outputs": {
        "concatOutput": {
            "value": "[concat(parameters('prefix'), uniqueString(resourceGroup().id))]",
            "type" : "string"
        }
    }
}
```

L'esempio seguente illustra come combinare due matrici.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { 
        "firstArray": { 
            "type": "array", 
            "defaultValue": [ 
                "1-1", 
                "1-2", 
                "1-3" 
            ] 
        },
        "secondArray": {
            "type": "array", 
            "defaultValue": [ 
                "2-1", 
                "2-2",
                "2-3" 
            ] 
        }
    },
    "resources": [
    ],
    "outputs": {
        "return": {
            "type": "array",
            "value": "[concat(parameters('firstArray'), parameters('secondArray'))]"
        }
    }
}
```

### <a name="return-value"></a>Valore restituito
Stringa o matrice di valori concatenati.

<a id="contains" />

## <a name="contains"></a>contains
`contains (container, itemToFind)`

Controlla se una matrice contiene un valore, se un oggetto contiene una chiave o se una stringa contiene una sottostringa.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| Contenitore |Sì |Matrice, oggetto o stringa |Valore che contiene il valore da trovare. |
| itemToFind |Sì |Stringa o numero intero |Valore da trovare. |

### <a name="examples"></a>esempi

L'esempio seguente illustra come usare il parametro contains con tipi differenti:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "OneTwoThree"
        },
        "objectToTest": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b", "three": "c"}
        },
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "stringTrue": {
            "type": "bool",
            "value": "[contains(parameters('stringToTest'), 'e')]"
        },
        "stringFalse": {
            "type": "bool",
            "value": "[contains(parameters('stringToTest'), 'z')]"
        },
        "objectTrue": {
            "type": "bool",
            "value": "[contains(parameters('objectToTest'), 'one')]"
        },
        "objectFalse": {
            "type": "bool",
            "value": "[contains(parameters('objectToTest'), 'a')]"
        },
        "arrayTrue": {
            "type": "bool",
            "value": "[contains(parameters('arrayToTest'), 'three')]"
        },
        "arrayFalse": {
            "type": "bool",
            "value": "[contains(parameters('arrayToTest'), 'four')]"
        }
    }
}
```

### <a name="return-value"></a>Valore restituito

**True** se l'elemento viene individuato; in caso contrario, restituisce **False**.

<a id="datauri" />

## <a name="datauri"></a>dataUri
`dataUri(stringToConvert)`

Converte un valore in un URI di dati.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| stringToConvert |Sì |string |Valore da convertire in un URI di dati. |

### <a name="examples"></a>esempi

L'esempio seguente converte un valore in un URI di dati e converte un URI di dati in una stringa:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "Hello"
        },
        "dataFormattedString": {
            "type": "string",
            "defaultValue": "data:;base64,SGVsbG8sIFdvcmxkIQ=="
        }
    },
    "resources": [],
    "outputs": {
        "dataUriOutput": {
            "value": "[dataUri(parameters('stringToTest'))]",
            "type" : "string"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[dataUriToString(parameters('dataFormattedString'))]"
        }
    }
}
```

### <a name="return-value"></a>Valore restituito

Stringa formattata come URI di dati.

<a id="datauritostring" />

## <a name="datauritostring"></a>dataUriToString
`dataUriToString(dataUriToConvert)`

Converte un valore formattato come URI di dati in una stringa.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| dataUriToConvert |Sì |string |Valore dell'URI di dati da convertire. |

### <a name="examples"></a>esempi

L'esempio seguente converte un valore in un URI di dati e converte un URI di dati in una stringa:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringToTest": {
            "type": "string",
            "defaultValue": "Hello"
        },
        "dataFormattedString": {
            "type": "string",
            "defaultValue": "data:;base64,SGVsbG8sIFdvcmxkIQ=="
        }
    },
    "resources": [],
    "outputs": {
        "dataUriOutput": {
            "value": "[dataUri(parameters('stringToTest'))]",
            "type" : "string"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[dataUriToString(parameters('dataFormattedString'))]"
        }
    }
}
```

### <a name="return-value"></a>Valore restituito

Stringa contenente il valore convertito.

<a id="empty" /> 

## <a name="empty"></a>empty
`empty(itemToTest)`

Determina se una matrice, un oggetto o una stringa è vuota.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| itemToTest |Sì |Matrice, oggetto o stringa |Valore da verificare se l'elemento è vuoto. |

### <a name="examples"></a>esempi

L'esempio seguente controlla se una matrice, un oggetto e una stringa sono vuoti.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": []
        },
        "testObject": {
            "type": "object",
            "defaultValue": {}
        },
        "testString": {
            "type": "string",
            "defaultValue": ""
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testArray'))]"
        },
        "objectEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testObject'))]"
        },
        "stringEmpty": {
            "type": "bool",
            "value": "[empty(parameters('testString'))]"
        }
    }
}
```

### <a name="return-value"></a>Valore restituito

**True** se il valore è vuoto; in caso contrario, restituisce **False**.

<a id="endswith" />

## <a name="endswith"></a>endsWith
`endsWith(stringToSearch, stringToFind)`

Determina se una stringa termina con un valore. Il confronto non fa distinzione tra maiuscole e minuscole.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| stringToSearch |Sì |string |Valore che contiene l'elemento da cercare. |
| stringToFind |Sì |string |Valore da trovare. |

### <a name="examples"></a>esempi

L'esempio seguente mostra come usare le funzioni startsWith ed endsWith:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "startsTrue": {
            "value": "[startsWith('abcdef', 'ab')]",
            "type" : "bool"
        },
        "startsCapTrue": {
            "value": "[startsWith('abcdef', 'A')]",
            "type" : "bool"
        },
        "startsFalse": {
            "value": "[startsWith('abcdef', 'e')]",
            "type" : "bool"
        },
        "endsTrue": {
            "value": "[endsWith('abcdef', 'ef')]",
            "type" : "bool"
        },
        "endsCapTrue": {
            "value": "[endsWith('abcdef', 'F')]",
            "type" : "bool"
        },
        "endsFalse": {
            "value": "[endsWith('abcdef', 'e')]",
            "type" : "bool"
        }
    }
}
```

### <a name="return-value"></a>Valore restituito

**True** se l'ultimo carattere o i caratteri della stringa corrispondono al valore; in caso contrario, restituisce **False**.

<a id="first" />

## <a name="first"></a>first
`first(arg1)`

Restituisce il primo carattere della stringa o il primo elemento della matrice.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| arg1 |Sì |Stringa o matrice |Valore per recuperare il primo elemento o carattere. |

### <a name="examples"></a>esempi

L'esempio seguente mostra come usare la prima funzione con una matrice e una stringa.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayOutput": {
            "type": "string",
            "value": "[first(parameters('arrayToTest'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[first('One Two Three')]"
        }
    }
}
```

### <a name="return-value"></a>Valore restituito

Stringa del primo carattere o il tipo del primo elemento in una matrice (stringa, numero intero, matrice o oggetto).

<a id="indexof" />

## <a name="indexof"></a>indexOf
`indexOf(stringToSearch, stringToFind)`

Restituisce la prima posizione di un valore all'interno di una stringa. Il confronto non fa distinzione tra maiuscole e minuscole.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| stringToSearch |Sì |string |Valore che contiene l'elemento da cercare. |
| stringToFind |Sì |string |Valore da trovare. |

### <a name="examples"></a>esempi

L'esempio seguente mostra come usare le funzioni indexOf e lastIndexOf:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "firstT": {
            "value": "[indexOf('test', 't')]",
            "type" : "int"
        },
        "lastT": {
            "value": "[lastIndexOf('test', 't')]",
            "type" : "int"
        },
        "firstString": {
            "value": "[indexOf('abcdef', 'CD')]",
            "type" : "int"
        },
        "lastString": {
            "value": "[lastIndexOf('abcdef', 'AB')]",
            "type" : "int"
        },
        "notFound": {
            "value": "[indexOf('abcdef', 'z')]",
            "type" : "int"
        }
    }
}
```

### <a name="return-value"></a>Valore restituito

Numero intero che rappresenta la posizione dell'elemento da trovare. Il valore è in base zero. Se l'elemento non viene trovato, viene restituito -1.


<a id="last" />

## <a name="last"></a>last
`last (arg1)`

Restituisce il primo carattere della stringa o l'ultimo elemento della matrice.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| arg1 |Sì |Stringa o matrice |Valore per recuperare l'ultimo elemento o carattere. |

### <a name="examples"></a>esempi

L'esempio seguente mostra come usare l'ultima funzione con una matrice e una stringa.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": ["one", "two", "three"]
        }
    },
    "resources": [
    ],
    "outputs": {
        "arrayOutput": {
            "type": "string",
            "value": "[last(parameters('arrayToTest'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[last('One Two Three')]"
        }
    }
}
```

### <a name="return-value"></a>Valore restituito

Stringa dell'ultimo carattere o il tipo dell'ultimo elemento in una matrice (stringa, numero intero, matrice o oggetto).

<a id="lastindexof" />

## <a name="lastindexof"></a>lastIndexOf
`lastIndexOf(stringToSearch, stringToFind)`

Restituisce l'ultima posizione di un valore all'interno di una stringa. Il confronto non fa distinzione tra maiuscole e minuscole.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| stringToSearch |Sì |string |Valore che contiene l'elemento da cercare. |
| stringToFind |Sì |string |Valore da trovare. |

### <a name="examples"></a>esempi

L'esempio seguente mostra come usare le funzioni indexOf e lastIndexOf:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "firstT": {
            "value": "[indexOf('test', 't')]",
            "type" : "int"
        },
        "lastT": {
            "value": "[lastIndexOf('test', 't')]",
            "type" : "int"
        },
        "firstString": {
            "value": "[indexOf('abcdef', 'CD')]",
            "type" : "int"
        },
        "lastString": {
            "value": "[lastIndexOf('abcdef', 'AB')]",
            "type" : "int"
        },
        "notFound": {
            "value": "[indexOf('abcdef', 'z')]",
            "type" : "int"
        }
    }
}
```

### <a name="return-value"></a>Valore restituito

Numero intero che rappresenta l'ultima posizione dell'elemento da trovare. Il valore è in base zero. Se l'elemento non viene trovato, viene restituito -1.


<a id="length" />

## <a name="length"></a>length
`length(string)`

Restituisce il numero di caratteri in una stringa o di elementi in una matrice.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| arg1 |Sì |Stringa o matrice |Matrice da usare per ottenere il numero di elementi oppure stringa da usare per ottenere il numero di caratteri. |

### <a name="examples"></a>esempi

L'esempio seguente mostra come usare la funzione length con una matrice e una stringa:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "arrayToTest": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "stringToTest": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "arrayLength": {
            "type": "int",
            "value": "[length(parameters('arrayToTest'))]"
        },
        "stringLength": {
            "type": "int",
            "value": "[length(parameters('stringToTest'))]"
        }
    }
}
```

### <a name="return-value"></a>Valore restituito

Numero intero 

<a id="padleft" />

## <a name="padleft"></a>padLeft
`padLeft(valueToPad, totalLength, paddingCharacter)`

Restituisce una stringa allineata a destra mediante l'aggiunta di caratteri a sinistra, fino a raggiungere la lunghezza totale specificata.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| valueToPad |Sì |Stringa o numero intero |Il valore per eseguire l'allineamento a destra. |
| totalLength |Sì |int |Numero totale di caratteri della stringa restituita. |
| paddingCharacter |No |Carattere singolo |Il carattere da utilizzare per la spaziatura interna a sinistra, fino a raggiungere la lunghezza totale. Il valore predefinito è uno spazio. |

Se la stringa originale è più lunga rispetto al numero di caratteri di riempimento, non vengono aggiunti caratteri.

### <a name="examples"></a>esempi

L'esempio seguente mostra come il valore del parametro fornito dall'utente viene completato aggiungendo il carattere zero finché la stringa non raggiunge il numero totale di caratteri. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "123"
        },
        "totalCharacters": {
            "type": "int",
            "defaultValue": 10
        }
    },
    "resources": [],
    "outputs": {
        "stringOutput": {
            "type": "string",
            "value": "[padLeft(parameters('testString'),parameters('totalCharacters'),'0')]"
        }
    }
}
```

### <a name="return-value"></a>Valore restituito

Stringa contenente come minimo il numero di caratteri specificati.

<a id="replace" />

## <a name="replace"></a>replace
`replace(originalString, oldCharacter, newCharacter)`

Restituisce una nuova stringa con tutte le istanze di un carattere della stringa specificata sostituito con un altro carattere.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| originalString |Sì |string |Il valore in cui tutte le istanze di un carattere vengono sostituite con un altro carattere. |
| oldCharacter |Sì |string |Carattere da rimuovere dalla stringa originale. |
| newCharacter |Sì |string |Carattere da aggiungere al posto del carattere rimosso. |

### <a name="examples"></a>esempi

Nell'esempio seguente viene illustrato come rimuovere tutti i trattini dalla stringa fornita dall'utente.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "123-123-1234"
        }
    },
    "resources": [],
    "outputs": {
        "stringOutput": {
            "type": "string",
            "value": "[replace(parameters('testString'),'-', '')]"
        }
    }
}
```

### <a name="return-value"></a>Valore restituito

Stringa con i caratteri sostituiti.

<a id="skip" />

## <a name="skip"></a>skip
`skip(originalValue, numberToSkip)`

Restituisce una stringa con tutti i caratteri dopo il numero specificato di caratteri o una matrice con tutti gli elementi dopo il numero specificato di elementi.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| originalValue |Sì |Stringa o matrice |Stringa o matrice da usare per i valori da ignorare. |
| numberToSkip |Sì |int |Numero di elementi o caratteri da ignorare. Se il valore è minore o uguale a 0, vengono restituiti tutti gli elementi o i caratteri nel valore. Se il valore è maggiore della lunghezza della stringa o della matrice, viene restituita una stringa o una matrice vuota. |

### <a name="examples"></a>esempi

L'esempio seguente ignora il numero specificato di elementi in una matrice e il numero specificato di caratteri in una stringa.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "elementsToSkip": {
            "type": "int",
            "defaultValue": 2
        },
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        },
        "charactersToSkip": {
            "type": "int",
            "defaultValue": 4
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "array",
            "value": "[skip(parameters('testArray'),parameters('elementsToSkip'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[skip(parameters('testString'),parameters('charactersToSkip'))]"
        }
    }
}
```

### <a name="return-value"></a>Valore restituito

Stringa o matrice.

<a id="split" />

## <a name="split"></a>split
`split(inputString, delimiter)`

Restituisce una matrice di stringhe che contiene le sottostringhe della stringa di input delimitate dai delimitatori specificati.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| inputString |Sì |string |Stringa da dividere. |
| delimiter |Sì |Stringa o matrice di stringhe |Il delimitatore da usare per dividere la stringa. |

### <a name="examples"></a>esempi

L'esempio seguente mostra come suddividere la stringa di input usando una virgola, oppure una virgola o un punto e virgola.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "firstString": {
            "type": "string",
            "defaultValue": "one,two,three"
        },
        "secondString": {
            "type": "string",
            "defaultValue": "one;two,three"
        }
    },
    "variables": {
        "delimiters": [ ",", ";" ]
    },
    "resources": [],
    "outputs": {
        "firstOutput": {
            "type": "array",
            "value": "[split(parameters('firstString'),',')]"
        },
        "secondOutput": {
            "type": "array",
            "value": "[split(parameters('secondString'),variables('delimiters'))]"
        }
    }
}
```

### <a name="return-value"></a>Valore restituito

Matrice di stringhe.

<a id="startswith" />

## <a name="startswith"></a>startsWith
`startsWith(stringToSearch, stringToFind)`

Determina se una stringa inizia con un valore. Il confronto non fa distinzione tra maiuscole e minuscole.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| stringToSearch |Sì |string |Valore che contiene l'elemento da cercare. |
| stringToFind |Sì |string |Valore da trovare. |

### <a name="examples"></a>esempi

L'esempio seguente mostra come usare le funzioni startsWith ed endsWith:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "startsTrue": {
            "value": "[startsWith('abcdef', 'ab')]",
            "type" : "bool"
        },
        "startsCapTrue": {
            "value": "[startsWith('abcdef', 'A')]",
            "type" : "bool"
        },
        "startsFalse": {
            "value": "[startsWith('abcdef', 'e')]",
            "type" : "bool"
        },
        "endsTrue": {
            "value": "[endsWith('abcdef', 'ef')]",
            "type" : "bool"
        },
        "endsCapTrue": {
            "value": "[endsWith('abcdef', 'F')]",
            "type" : "bool"
        },
        "endsFalse": {
            "value": "[endsWith('abcdef', 'e')]",
            "type" : "bool"
        }
    }
}
```

### <a name="return-value"></a>Valore restituito

**True** se il primo carattere o i caratteri della stringa corrispondono al valore; in caso contrario, restituisce **False**.


<a id="string" />

## <a name="string"></a>string
`string(valueToConvert)`

Converte il valore specificato in una stringa.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| valueToConvert |Sì | Qualsiasi |Valore da convertire in stringa. È possibile convertire qualsiasi tipo di valore, inclusi gli oggetti e le matrici. |

### <a name="examples"></a>esempi

L'esempio seguente mostra come convertire diversi tipi di valori di stringa:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testObject": {
            "type": "object",
            "defaultValue": {
                "valueA": 10,
                "valueB": "Example Text"
            }
        },
        "testArray": {
            "type": "array",
            "defaultValue": [
                "a",
                "b",
                "c"
            ]
        },
        "testInt": {
            "type": "int",
            "defaultValue": 5
        }
    },
    "resources": [],
    "outputs": {
        "objectOutput": {
            "type": "string",
            "value": "[string(parameters('testObject'))]"
        },
        "arrayOutput": {
            "type": "string",
            "value": "[string(parameters('testArray'))]"
        },
        "intOutput": {
            "type": "string",
            "value": "[string(parameters('testInt'))]"
        }
    }
}
```

### <a name="return-value"></a>Valore restituito

Stringa.

<a id="substring" />

## <a name="substring"></a>substring
`substring(stringToParse, startIndex, length)`

Restituisce una sottostringa che inizia nella posizione del carattere specificato e contiene il numero di caratteri specificato.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| stringToParse |Sì |string |La stringa originale da cui estrarre la sottostringa. |
| startIndex |No |int |La posizione del carattere iniziale in base zero della sottostringa. |
| length |No |int |Il numero di caratteri della sottostringa. Deve fare riferimento a una posizione nella stringa. |

### <a name="examples"></a>esempi

L'esempio seguente estrae una sottostringa da un parametro.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        }
    },
    "resources": [],
    "outputs": {
        "substringOutput": {
            "value": "[substring(parameters('testString'), 4, 3)]",
            "type": "string"
        }
    }
}
```

L'esempio seguente ha esito negativo e visualizza l'errore "I parametri index e length devono fare riferimento a una posizione all'interno della stringa. Parametro index: '0', parametro length: '11', lunghezza del parametro string: '10'.".

```json
"parameters": {
    "inputString": { "type": "string", "value": "1234567890" }
},
"variables": { 
    "prefix": "[substring(parameters('inputString'), 0, 11)]"
}
```

<a id="take" />

## <a name="take"></a>take
`take(originalValue, numberToTake)`

Restituisce una stringa con il numero specificato di caratteri dall'inizio della stringa o una matrice con il numero specificato di elementi dall'inizio della matrice.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| originalValue |Sì |Stringa o matrice |Stringa o matrice da cui prendere gli elementi. |
| numberToTake |Sì |int |Numero di elementi o caratteri da prendere. Se il valore è minore o uguale a 0, viene restituita una stringa o un matrice vuota. Se il valore è maggiore della lunghezza della stringa o matrice specificata, vengono restituiti tutti gli elementi nella stringa o nella matrice. |

### <a name="examples"></a>esempi

L'esempio seguente prende il numero specificato di elementi dalla matrice e di caratteri dalla stringa.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testArray": {
            "type": "array",
            "defaultValue": [
                "one",
                "two",
                "three"
            ]
        },
        "elementsToTake": {
            "type": "int",
            "defaultValue": 2
        },
        "testString": {
            "type": "string",
            "defaultValue": "one two three"
        },
        "charactersToTake": {
            "type": "int",
            "defaultValue": 2
        }
    },
    "resources": [],
    "outputs": {
        "arrayOutput": {
            "type": "array",
            "value": "[take(parameters('testArray'),parameters('elementsToTake'))]"
        },
        "stringOutput": {
            "type": "string",
            "value": "[take(parameters('testString'),parameters('charactersToTake'))]"
        }
    }
}
```

### <a name="return-value"></a>Valore restituito

Stringa o matrice.

<a id="tolower" />

## <a name="tolower"></a>toLower
`toLower(stringToChange)`

Converte la stringa specificata in caratteri minuscoli.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| stringToChange |Sì |string |Il valore da convertire in lettere minuscole. |

### <a name="examples"></a>esempi

L'esempio seguente converte il valore del parametro in lettere maiuscole e minuscole.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "toLowerOutput": {
            "value": "[toLower(parameters('testString'))]",
            "type": "string"
        },
        "toUpperOutput": {
            "type": "string",
            "value": "[toUpper(parameters('testString'))]"
        }
    }
}
```

<a id="toupper" />

## <a name="toupper"></a>toUpper
`toUpper(stringToChange)`

Converte la stringa specificata in lettere maiuscole.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| stringToChange |Sì |string |Il valore da convertire in lettere maiuscole. |

### <a name="examples"></a>esempi

L'esempio seguente converte il valore del parametro in lettere maiuscole e minuscole.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "One Two Three"
        }
    },
    "resources": [],
    "outputs": {
        "toLowerOutput": {
            "value": "[toLower(parameters('testString'))]",
            "type": "string"
        },
        "toUpperOutput": {
            "type": "string",
            "value": "[toUpper(parameters('testString'))]"
        }
    }
}
```

<a id="trim" />

## <a name="trim"></a>Trim
`trim (stringToTrim)`

Rimuove tutti i caratteri di spazi vuoti iniziali e finali dalla stringa specificata.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| stringToTrim |Sì |string |Il valore da tagliare. |

### <a name="examples"></a>esempi

L'esempio seguente elimina i caratteri spazi vuoti dal parametro.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "testString": {
            "type": "string",
            "defaultValue": "    one two three   "
        }
    },
    "resources": [],
    "outputs": {
        "return": {
            "type": "string",
            "value": "[trim(parameters('testString'))]"
        }
    }
}
```

<a id="uniquestring" />

## <a name="uniquestring"></a>uniqueString
`uniqueString (baseString, ...)`

Crea una stringa hash deterministica in base ai valori forniti come parametri. 

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| baseString |Sì |string |Il valore usato nella funzione hash per creare una stringa univoca. |
| parametri aggiuntivi in base alle esigenze |No |string |È possibile aggiungere tutte le stringhe necessarie per creare il valore che specifica il livello di univocità. |

### <a name="remarks"></a>Osservazioni

Questa funzione è utile quando è necessario creare un nome univoco per una risorsa. È possibile specificare i valori dei parametri che limitano l'ambito di univocità per il risultato. È possibile specificare se il nome è univoco nella sottoscrizione, nel gruppo di risorse o nella distribuzione. 

Il valore restituito non è una stringa casuale, ma il risultato di una funzione hash. Il valore restituito ha una lunghezza di 13 caratteri. Non è globalmente univoco. È possibile combinare il valore con un prefisso dalla convenzione di denominazione scelta per creare un nome significativo. L'esempio seguente illustra il formato del valore restituito. Il valore effettivo varia in base ai parametri forniti.

    tcvhiyu5h2o5o

### <a name="examples"></a>esempi

Gli esempi seguenti mostrano come usare uniqueString per creare un valore univoco per livelli di uso comune.

Con ambito univoco nella sottoscrizione

```json
"[uniqueString(subscription().subscriptionId)]"
```

Con ambito univoco nel gruppo di risorse

```json
"[uniqueString(resourceGroup().id)]"
```

Con ambito univoco nella distribuzione per un gruppo di risorse

```json
"[uniqueString(resourceGroup().id, deployment().name)]"
```

Nell'esempio seguente viene illustrato come creare un nome univoco per un account di archiviazione in base al gruppo di risorse. All'interno del gruppo di risorse, il nome non è univoco se costruito allo stesso modo.

```json
"resources": [{ 
    "name": "[concat('storage', uniqueString(resourceGroup().id))]", 
    "type": "Microsoft.Storage/storageAccounts", 
    ...
```

### <a name="return-value"></a>Valore restituito

Stringa contenente 13 caratteri

<a id="uri" />

## <a name="uri"></a>Uri
`uri (baseUri, relativeUri)`

Crea un URI assoluto combinando la baseUri e la stringa relativeUri.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| baseUri |Sì |string |La stringa URI di base. |
| relativeUri |Sì |string |La stringa URI relativa da aggiungere alla stringa di URI di base. |

Il valore per il parametro **baseUri** può includere un file specifico, ma solo il percorso di base viene usato per costruire l'URI. Ad esempio, se si trasmette `http://contoso.com/resources/azuredeploy.json` come parametro baseUri, viene restituito l'URI di base `http://contoso.com/resources/`.

### <a name="examples"></a>esempi

Nell'esempio seguente viene illustrato come costruire un collegamento a un modello annidato in base al valore del modello padre.

```json
"templateLink": "[uri(deployment().properties.templateLink.uri, 'nested/azuredeploy.json')]"
```

L'esempio seguente mostra come usare uri, uriComponent e uriComponentToString:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

### <a name="return-value"></a>Valore restituito

Stringa che rappresenta l'URI assoluto dei valori di base e relativi.

<a id="uricomponent" />

## <a name="uricomponent"></a>uriComponent
`uricomponent(stringToEncode)`

Codifica un URI.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| stringToEncode |Sì |string |Valore da codificare. |

### <a name="examples"></a>esempi

L'esempio seguente mostra come usare uri, uriComponent e uriComponentToString:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

### <a name="return-value"></a>Valore restituito

Stringa del valore URI codificato.

<a id="uricomponenttostring" />

## <a name="uricomponenttostring"></a>uriComponentToString
`uriComponentToString(uriEncodedString)`

Restituisce una stringa di un valore URI codificato.

### <a name="parameters"></a>Parametri

| Parametro | Obbligatorio | Tipo | Descrizione |
|:--- |:--- |:--- |:--- |
| uriEncodedString |Sì |string |Valore URI codificato da convertire in stringa. |

### <a name="examples"></a>esempi

L'esempio seguente mostra come usare uri, uriComponent e uriComponentToString:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
        "uriFormat": "[uri('http://contoso.com/resources/', 'nested/azuredeploy.json')]",
        "uriEncoded": "[uriComponent(variables('uriFormat'))]" 
    },
    "resources": [
    ],
    "outputs": {
        "uriOutput": {
            "type": "string",
            "value": "[variables('uriFormat')]"
        },
        "componentOutput": {
            "type": "string",
            "value": "[variables('uriEncoded')]"
        },
        "toStringOutput": {
            "type": "string",
            "value": "[uriComponentToString(variables('uriEncoded'))]"
        }
    }
}
```

### <a name="return-value"></a>Valore restituito

Stringa decodificata del valore URI codificato.

## <a name="next-steps"></a>Passaggi successivi
* Per una descrizione delle sezioni in un modello Azure Resource Manager, vedere [Creazione di modelli di Azure Resource Manager](resource-group-authoring-templates.md).
* Per unire più modelli, vedere [Uso di modelli collegati con Azure Resource Manager](resource-group-linked-templates.md).
* Per eseguire un'iterazione di un numero di volte specificato durante la creazione di un tipo di risorsa, vedere [Creare più istanze di risorse in Gestione risorse di Azure](resource-group-create-multiple.md).
* Per informazioni su come distribuire il modello che è stato creato, vedere [Distribuire un'applicazione con un modello di Azure Resource Manager](resource-group-template-deploy.md).


