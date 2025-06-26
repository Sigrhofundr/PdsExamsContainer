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

**Risposte**

**Parte I**

* La funzione `ram_stealmem` alloca la memoria in modo incrementale a partire dal primo indirizzo disponibile dopo il caricamento del kernel. **Vero** - Dopo aver calcolato lo spazio richiesto, somma quest'ultimo al primo indirizzo disponibile e verifica se questo ha un valore compatibile;
* Lo schema di allocazione utilizzato da `ram_stealmem` non consentirà l'alternanza di partizioni contigue di RAM fisica del kernel e dell'utente (in altre parole, le partizioni del kernel e dell'utente non saranno mai alternate). **Falso** - Anche se l’allocazione è incrementale, in OS161 non c’è una separazione rigida tra memoria del kernel e memoria utente fisica: entrambe possono essere allocate in sequenza e quindi, se si alternano richieste di kernel e utente, le partizioni fisiche possono essere alternate.
* La funzione `ram_stealmem` viene chiamata per l'allocazione della memoria sia del kernel (memoria dinamica) che dei processi utente. **Vero** - `ram_stealmem` viene usata sia per allocare memoria dinamica per il kernel che per assegnare pagine fisiche agli spazi d’indirizzamento utente, almeno fino all’inizializzazione di un gestore di memoria più evoluto.
* Anche se la soluzione proposta per supportare la deallocazione della memoria (`freeppages`) si basa su una bitmap, si potrebbe utilizzare anche una lista collegata. **Vero** -     Una lista collegata di blocchi liberi è un’alternativa classica alla bitmap per gestire la memoria fisica: entrambe sono strutture dati valide per la gestione di allocazione/deallocazione.
* Quando si utilizza una bitmap per tenere traccia della RAM allocata/libera, è necessario un secondo array che memorizzi le dimensioni delle partizioni allocate per l'allocazione sia della memoria dinamica del kernel che dei processi utente. **Falso** - Se l’allocazione/deallocazione avviene sempre a livello di pagina, la bitmap è sufficiente. Un array per le dimensioni serve solo se si supportano blocchi di dimensione variabile, ma in OS161 dumbvm la granularità è la pagina.
* Lo stack di un processo utente non viene allocato (in `dumbvm`) chiamando `ram_stealmem`, perché la dimensione dello stack è una costante e lo stack si trova sempre agli stessi indirizzi logici. **Falso** - Anche se la dimensione logica dello stack è costante e l’indirizzo virtuale è fisso, la memoria fisica per lo stack viene comunque allocata (tipicamente tramite ram_stealmem) quando si crea lo spazio d’indirizzamento del processo.

**Parte II**

Dato il seguente indirizzo logico (potrebbe essere sia utente che kernel), convertirlo nel relativo indirizzo fisico: 
1. `0x8110`
2. `0x6500`
3. `0x7FFFE010`
4. `0x805000B0`.