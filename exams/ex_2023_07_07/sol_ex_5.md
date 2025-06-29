### Domanda 5: Implementazioni di `spinlock_acquire` in OS161

Considera, in OS161, le due possibili implementazioni di `spinlock_acquire` mostrate di seguito:

```c
void spinlock_acquire(struct spinlock *splk) {
 // ...
 while (1) {
    if (spinlock_data_get(&splk->splk_lock) != 0) {
        continue;
    }
    if (spinlock_data_testandset(&splk->splk_lock) != 0) {
        continue;
    }
    break;
 }
 // ...
}

void spinlock_acquire2 (struct spinlock *splk) {
 //
 while (1) {
    while (spinlock_data_get(&splk->splk_lock) != 0);
    if (spinlock_data_testandset(&splk->splk_lock) == 0) {
    continue;
    }
 }
 //
}
```

A) Perché `spinlock_acquire` utilizza sia `spinlock_data_get` che `spinlock_data_testandset`, invece di chiamarne solo una?
B) `spinlock_acquire2` è equivalente a `spinlock_acquire`? Se Sì, motiva, se No, motiva e, se possibile, modificala per renderla equivalente

---

**Risposte**

A) OS161 utilizza il pattern test-test-and-set per ottimizzare le performance. `spinlock_data_get` fornisce una lettura veloce e non atomica per verificare se il lock è libero, 
riducendo la bus contention. Solo quando il lock appare disponibile, viene chiamata la costosa operazione atomica `spinlock_data_testandset` per l'acquisizione effettiva. 
Questo evita il traffico continuo sul bus di sistema che si avrebbe usando solo test-and-set.

B) `spinlock_acquire2` non è equivalente per due motivi: loop infinito nella fase di test e logica invertita nella condizione di uscita. 
La correzione richiede sia un loop esterno che la correzione della condizione di break.

```c
void spinlock_acquire2(struct spinlock *splk) {
//
    while (1) {
        while (spinlock_data_get(&splk->splk_lock) != 0);
        if (spinlock_data_testandset(&splk->splk_lock) == 0) {
            break;  // Esce quando acquisisce il lock
        }
        // Se testandset fallisce, riprova tutto
    }
    //
}
```

Le due versioni sono funzionalmente equivalenti (entrambe acquisiscono correttamente il lock), ma hanno comportamenti leggermente diversi nella gestione delle race condition.