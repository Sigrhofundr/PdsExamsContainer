## Domanda 2 (comprende le domande da 5 a 8 del questionario)

Si consideri un file system basato su allocazione indicizzata, in cui i blocchi di indice sono organizzati su due livelli (indici gerarchici).
**ATTENZIONE:** questa non è un'organizzazione Unix-UFS basata su INODE, ma solo su due livelli di indice!
I puntatori/indici hanno una dimensione di 32 bit, i blocchi del disco hanno una dimensione di 4 KB e il file system risiede su una partizione del disco da 800 GB, che include sia blocchi di dati che blocchi di indice.

### Domanda 5

**Punteggio max.: 0,75**

Calcolare la dimensione massima possibile per un file.

**Risposta:** 4,294967296 GB

* Dimensione blocco: 4 KB = 4096 byte
* Dimensione puntatore: 32 bit = 4 byte
* Puntatori per blocco: 4096 / 4 = 1024 puntatori

Il blocco di primo livello contiene 1024 puntatori
Ogni puntatore punta a un blocco di secondo livello
Ogni blocco di secondo livello contiene 1024 puntatori a blocchi dati
Totale blocchi dati: 1024 × 1024 = 1.048.576 blocchi

Dim_max_file = Blocchi dati × Dimensione blocco = 1.048.576 × 4 KB = 4.194.304 KB = 4.096 MB = 4 GB

---

### Domanda 6

**Punteggio max.: 0,75**

Dato un file binario di 5381KB, calcolare esattamente quanti blocchi indice occupa il file.

**Risposta:** 3

Dimensione file: 5381 KB
Dimensione blocco: 4 KB
Blocchi dati = ⌈5381 / 4⌉ = ⌈1345.25⌉ = 1346 blocchi dati
Dobbiamo indirizzare 1346 blocchi dati
Ogni blocco di livello 2 può puntare a 1024 blocchi dati
Blocchi livello 2 = ⌈1346 / 1024⌉ = ⌈1.314⌉ = 2 blocchi

Livello 1: 1 blocco
Livello 2: 2 blocchi
Totale: 3 blocchi indice


---------
### Domanda 7

**Punteggio max.: 0,75**

Dato il file binario della domanda precedente, di dimensione 5381KB, calcolare la frammentazione interna relativa ai blocchi indice.  Il risultato va rappresentato in Byte (B).

**Risposta:** 3040

### Domanda 8

**Punteggio max.: 0,75**

Si sa che il file (di dimensione 5381KB) contiene una sequenza di numeri reali in formato double (64 bit per numero), calcolare a quanti blocchi di indice (complessivamente, a primo e secondo livello) si accede, per leggere i primi 256K (1 K significa 2^10) numeri.

**Risposta:**
