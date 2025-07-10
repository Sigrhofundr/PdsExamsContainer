## Domanda 2 (comprende le domande da 6 a 11 del questionario)

### Domanda 6

Si consideri un file system di tipo Unix, basato su inode, con 15 puntatori/indici (12 diretti, 1 singolo indiretto,
1 doppio indiretto e 1 triplo indiretto). I puntatori/indici hanno dimensione di 32 bit e i blocchi del disco hanno
dimensione di 4KB. Il file system risiede su una partizione del disco, dove 4 TB sono riservati ai blocchi di dati.
Lo spazio extra riservato ai metadati (inclusi i blocchi di indice) può essere trascurato ai fini di questo esercizio.

A) È noto che il numero di file è 512, con una dimensione media di 120 KB. È anche noto che il file più
piccolo nel file system ha una dimensione di 32 KB e il più grande di 400 KB. Calcolare il numero di blocchi di
dati utilizzati nel file system, come intervallo min-max (numero minimo e massimo di blocchi di dati).
Scrivere qui il numero minimo

**RISPOSTA REPORT PROF CABODI:** 4120

Min number of data blocks is when all files have 0 fragmentation.
average_size * num_files / block_size
La risposta corretta è : 15360,00 K

<!-- **Risposta**:

Riepiloghiamo:
* Dimensione blocco: 4KB = 4096 bytes
* Dimensione puntatore: 32 bit = 4 bytes
* Puntatori per blocco: 4096 / 4 = 1024 puntatori

Capacità di memorizzazione per tipo:

* Diretti: 12 × 4KB = 48KB
* Singolo indiretto: 1024 × 4KB = 4MB
* Doppio indiretto: 1024 × 1024 × 4KB = 4GB
* Triplo indiretto: 1024³ × 4KB = 4TB

Dim_tot_media = N_file* Dim_Media = 512 * 120 KB = 61.440 KB
Blocchi_min = ⌊61.440KB / 4KB⌋ = 15.360 blocchi
-->
---

### Domanda 7

Scrivere qui il numero massimo

**RISPOSTA REPORT PROF CABODI:**
max number of blocks is when all files have maximum fragmentation (1 block - 1 Byte, we approximate to 1
entire block):

min num + num_files
La risposta corretta è: 15872

<!--
**Risposta**: 15360

Blocchi_max = 512 × ⌈120KB / 4KB⌉ = 512 × 30 = 15360 blocchi
-->
---

### Domanda 8

Calcolare il numero minimo e massimo di file allocati con formato single indirect.

Scrivere il minimo qui

**RISPOSTA REPORT PROF CABODI:**

The maximum file of 400 KB is with single indirect with all block sizes.
For the minimum file size TWO CASES ARE POSSIBLE:
A) the minimun size of 32 KB is compatible with direct indexing (this is true for 4KB blocks)
B) the minimun size of 32 KB is compatible with single indexing (this is true for 2KB blocks)
In case A the result is 1 (as all files could be with direct except one): 0 is also accepted.
In case B all files are with single.
La risposta corretta è : 1

---

### Domanda 9

Calcolare il numero minimo e massimo di file allocati con formato single indirect.

Scrivere il massimo qui

**RISPOSTA REPORT PROF CABODI:**

We need to check id all files can be single indirect. If average size is in the range of possible sizes of single
indirect, then all files could be single indirect.
It is true with the given data
La risposta corretta è : 512
---

### Domanda 10

Dato un file binario di dimensione 90069 KB, calcolare esattamente quanti blocchi indice e blocchi dati
utilizza il file.

Scrivere il numero di blocchi DATO qui

**Risposta**: 22.518 blocchi **RISPOSTA REPORT PROF CABODI:(conferma)**

* Blocchi_dati = ⌈Dimensione_file / Dimensione_blocco⌉
* Blocchi_dati = ⌈90069 KB / 4 KB⌉
* Blocchi_dati = ⌈22.517,25⌉
* Blocchi_dati = 22.518 blocchi

---

### Domanda 11

Dato un file binario di dimensione 90069 KB, calcolare esattamente quanti blocchi indice e blocchi dati
utilizza il file.

Scrivere il numero di blocchi INDICE qui

**Risposta**: 23 blocchi **RISPOSTA REPORT PROF CABODI:** 23 (conferma)

- Blocchi_diretti = 12 blocchi (sempre utilizzati completamente)
- Dati_diretti = 12 * 4 KB = 48 KB
- Blocchi_single = 1024 blocchi (sempre utilizzati completamente)
- Dati_single = 1024 × 4 KB = 4096 KB
- Dati_rimanenti = 90,069KB - 48KB - 4,096KB = 85,925KB 
- Blocchi_double_dati = ⌈85,925KB / 4KB⌉ = 21,482 blocchi
- Blocchi_single_indirect_needed = ⌈21,482 / 1,024⌉ = 21 blocchi
- Blocchi_indice = 1 (single indirect) + 1 (double indirect) + 21 (single indirect del double)
- Blocchi_indice = 23 blocchi
---