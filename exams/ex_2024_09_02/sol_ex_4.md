### Domanda 4

#### Parte I
Si consideri un sistema OS161 in esecuzione su un simulatore sys161 MIPS con 8 MB di RAM.
La memoria fisica in `dumbvm` viene allocata chiamando la funzione `ram_stealmem`. Per ciascuna delle seguenti affermazioni, rispondere vero/falso (senza spiegazione).

* La funzione `ram_stealmem` alloca la memoria in modo incrementale a partire dal primo indirizzo disponibile dopo il caricamento del kernel.
* Lo schema di allocazione utilizzato da `ram_stealmem` non consentirà l'alternanza di partizioni contigue di RAM fisica del kernel e dell'utente (in altre parole, le partizioni del kernel e dell'utente non saranno mai alternate).
* La funzione `ram_stealmem` viene chiamata per l'allocazione della memoria sia del kernel (memoria dinamica) che dei processi utente.
* Anche se la soluzione proposta per supportare la deallocazione della memoria (`freeppages`) si basa su una bitmap, si potrebbe utilizzare anche una lista collegata.
* Quando si utilizza una bitmap per tenere traccia della RAM allocata/libera, è necessario un secondo array che memorizzi le dimensioni delle partizioni allocate per l'allocazione sia della memoria dinamica del kernel che dei processi utente.
* Lo stack di un processo utente non viene allocato (in `dumbvm`) chiamando `ram_stealmem`, perché la dimensione dello stack è una costante e lo stack si trova sempre agli stessi indirizzi logici.

#### Parte II

È noto che lo spazio degli indirizzi di un utente è caratterizzato da `as->as_pbase1`, `as->as_pbase2`, `as->as_vbase1`, `as->as_vbase2`, `as->as_npages1`, `as->as_npages2`, `as->as_stackpbase`, aventi i seguenti valori: `0x200000`, `0x300000`, `0x3000`, `0x7000`, `3`, `4`, `0x400000`. Le pagine in OS161 hanno una dimensione di 4 KB. È anche noto che in `dumbvm` viene allocato uno stack utente di 18 pagine.

Dato il seguente indirizzo logico (potrebbe essere sia utente che kernel), convertirlo nel relativo indirizzo fisico: `0x8110`, `0x6500`, `0x7FFFE010`, `0x805000B0`.

---