
### Domanda 4

Considera tre thread del kernel OS161 che implementano un'attività di trasferimento dati basata su un modello produttore/consumatore (2 produttori, 1 consumatore). I thread condividono un buffer di dati, implementato come una struttura C di tipo `struct prodConsBuf` definita come segue:

```c
struct prodConsBuf {
    data_t data;
    int dataReady;
    struct lock *pc_lk;
    struct cv *pc_cv;
};
```

Tutte le operazioni (scrittura/lettura) richiedono mutua esclusione. Quando non sono presenti dati nel buffer (quindi il buffer è vuoto), il flag `dataReady` è 0. Dopo che un produttore scrive nuovi dati nel buffer (campo `data`), imposta `dataReady` a 1 (quindi buffer pieno) e segnala il consumatore (il lavoro viene eseguito chiamando la funzione `producerWrite`).

```c
void producerWrite (struct prodConsBuf *pc, data_t *dp);
```

Il consumatore esegue iterativamente la funzione `consumerRead`.

```c
void consumerRead (struct prodConsBuf *pc, data_t *dp);
```

Rispondi alle seguenti domande:

A) La struttura condivisa può essere posizionata nello stack del thread, oppure dovrebbe essere una variabile globale o altro?
B) Poiché i campi `pc_lk` e `pc_cv` sono puntatori, dove dovrebbero essere chiamate le funzioni `lock_create()` e `cv_create()`? Nel thread produttore, nei thread consumatori? Altrove?
C) Le funzioni produttore e consumatore sono parzialmente fornite. Completa la loro implementazione dove indicato (`<1>`, `<2>` e `<3>`). Si prega di notare che dobbiamo evitare il *busy waiting*, quindi un produttore segnala il consumatore quando i dati sono pronti. Poiché un produttore potrebbe essere in attesa che il consumatore ottenga i dati precedenti, prima che al produttore sia consentito di scrivere nuovi dati, il consumatore deve anche inviare un segnale.

```c
/* le istruzioni di controllo/gestione degli errori sono omesse per semplicità */

void producerWrite (struct prodConsBuf *pc, data_t *dp) {
    /* lock per mutua esclusione */
    lock_acquire (pc->pc_lk);

    /* gestire correttamente qui il fatto che il buffer potrebbe essere pieno
       (riempito dall'altro produttore e non ancora letto dal consumatore) */
    /* <1> */

    /* copia dati: fatto qui con semplice assegnazione
       potrebbe essere usata invece una funzione apposita */
    pc->data = *dp;
    pc->dataReady = 1; /* imposta flag */

    /* segnala il consumatore */
    /* <2> */

    /* rilascia il lock e ritorna */
    lock_release (pc->pc_lk);
}

void consumerRead (struct prodConsBuf *pc, data_t *dp) {
    /* lock per mutua esclusione */
    lock_acquire (pc->pc_lk);

    while (!pc->dataReady) {
        cv_wait (pc->pc_cv, pc->pc_lk);
    }

    /* copia dati: fatto qui con semplice assegnazione */
    *dp = pc->data;
    pc->dataReady = 0; /* resetta flag */

    /* segnala un produttore in attesa, se presente */
    /* <3> */

    /* rilascia il lock e ritorna */
    lock_release (pc->pc_lk);
}
```

---
**Risposte**

>[!WARNING]
> Soluzione presa direttamente dal report del prof

A) Ogni thread ha il proprio stack, quindi un dato condiviso non può risiedere lì. Una variabile globale è l'opzione più comune. Altre opzioni includono l'allocazione dinamica (tramite `kmalloc`), se gestita correttamente.

B) Potrebbe essere fatto in ognuno di essi, a patto che si sincronizzino correttamente: ad esempio, un thread esegue le inizializzazioni, mentre gli altri thread attendono. 
Tuttavia, una soluzione più comune è che le inizializzazioni vengano eseguite da un altro thread, il thread genitore/master, che crea i produttori e i consumatori.

C)
```c
/* <1> attende che il buffer sia vuoto */
while (pc->dataReady) { 
    cv_wait(pc->pc_cv, pc->pc_lk); 
}

/* <2> è necessario cv_broadcast perché sia il consumatore sia l'altro produttore potrebbero essere in attesa, e in questo caso devono 
essere entrambi segnalati (con cv_signal il consumatore non è garantito che venga segnalato). Se vogliamo uno schema di sincronizzazione più specifico, abbiamo bisogno di due cv */
cv_broadcast(pc->pc_cv, pc->pc_lk);

**<3>** /* cv_signal è sufficiente perché nel caso di due produttori in attesa, uno dei due verrà segnalato */
cv_signal(pc->pc_cv, pc->pc_lk);
```