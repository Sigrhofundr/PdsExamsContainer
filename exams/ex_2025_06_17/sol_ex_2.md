## Domanda 2 (comprende le domande da 5 a 8 del questionario)

Si consideri un file system basato su allocazione indicizzata, in cui i blocchi di indice sono organizzati su due livelli (indici gerarchici).
**ATTENZIONE:** questa non è un'organizzazione Unix-UFS basata su INODE, ma solo su due livelli di indice!
I puntatori/indici hanno una dimensione di 32 bit, i blocchi del disco hanno una dimensione di 4 KB e il file system risiede su una partizione del disco da 800 GB, che include sia blocchi di dati che blocchi di indice.

### Domanda 5

**Punteggio max.: 0,75**

Calcolare la dimensione massima possibile per un file.

**Risposta:** 4,294967296 GB

---

### Domanda 6

**Punteggio max.: 0,75**

Dato un file binario di 5381KB, calcolare esattamente quanti blocchi indice occupa il file.

**Risposta:** 6

---------
### Domanda 7

**Punteggio max.: 0,75**

Dato il file binario della domanda precedente, di dimensione 5381KB, calcolare la frammentazione interna relativa ai blocchi indice.  Il risultato va rappresentato in Byte (B).

**Risposta:** 3040

### Domanda 8

**Punteggio max.: 0,75**

Si sa che il file (di dimensione 5381KB) contiene una sequenza di numeri reali in formato double (64 bit per numero), calcolare a quanti blocchi di indice (complessivamente, a primo e secondo livello) si accede, per leggere i primi 256K (1 K significa 2^10) numeri.

**Risposta:**
