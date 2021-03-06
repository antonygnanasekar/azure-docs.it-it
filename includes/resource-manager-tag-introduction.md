I tag vengono applicati alle risorse di Azure per organizzarle in modo logico in categorie. Ogni tag è costituito da una chiave e un valore. Ad esempio, è possibile applicare la chiave "Environment" e il valore "Production" a tutte le risorse nell'ambiente di produzione. Senza questo tag, potrebbe essere difficile stabilire se una risorsa è destinata allo sviluppo, al testing o alla produzione. "Environment" e "Production" sono solo esempi. È possibile definire le chiavi e i valori più adatti per organizzare la sottoscrizione.

Dopo aver applicato i tag, è possibile recuperare tutte le risorse nella sottoscrizione che hanno una chiave e un valore corrispondenti. I tag permettono di recuperare risorse correlate che si trovano in gruppi di risorse diversi. Questo approccio può risultare utile per l'organizzazione delle risorse per la fatturazione o la gestione.

Ai tag si applicano le limitazioni seguenti:

* Ogni risorsa o gruppo di risorse può avere un massimo di 15 coppie chiave-valore di tag. Questa limitazione si applica solo ai tag applicati direttamente al gruppo di risorse o alla risorsa. Un gruppo di risorse può contenere più risorse ognuna con 15 coppie chiave-valore di tag. 
* Il nome del tag non può superare i 512 caratteri.
* Il valore del tag non può superare i 256 caratteri. 
* I tag applicati al gruppo di risorse non vengono ereditati dalle risorse in tale gruppo di risorse. 

Se si devono associare più di 15 valori a una risorsa, usare una stringa JSON come valore di tag. La stringa JSON può contenere diversi valori applicati a una singola chiave di tag. In questo articolo è illustrato un esempio di assegnazione di una stringa JSON alla chiave del tag.