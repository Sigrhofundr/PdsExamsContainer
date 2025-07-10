## Domanda 4 (comprende le domande da 15 a 18 del questionario)

### Domanda 15 - Risposte con Sì/No alle affermazioni/domande

_Premessa:_
Si consideri, in OS161, l'implementazione della gestione della memoria virtuale.

A) Supponiamo di voler fare in dumbvm la gestione dei frame liberi tramite free-list, invece che con bitmap
(come fatto nella soluzione proposta per il lab 2)

_Domande:_
* La free list dovrebbe essere utilizzata solo per il segmento di codice e il segmento di dati di un processo, non per
  lo stack utente, poiché lo stack ha una dimensione fissa in dumbvm **RISPOSTA REPORT PROF CABODI:** NO
* occorre supportare la ricerca di intervalli di frame liberi contigui **RISPOSTA REPORT PROF CABODI:** YES
* non è necessaria la ricerca di frame liberi contigui, perché MIPS ha una TLB **RISPOSTA REPORT PROF CABODI:** NO
* La free-list può essere organizzata efficacemente come una lista di intervalli contigui di frame liberi **RISPOSTA REPORT PROF CABODI:** YES

---

### Domanda 16

_Premessa:_
L’allocazione di memoria per un processo utente in dumbvm comprende chiamata/e alla funzione as_define_region,
parzialmente elencata di seguito
```c
// vm.h includes the definition
// #define PAGE_FRAME 0xfffff000

int as_define_region(struct addrspace *as, vaddr_t vaddr, size_t sz,
int readable, int writeable, int executable) {
    size_t npages;
    ...
    /* Align the region. First, the base... */
    sz += vaddr & ~(vaddr_t)PAGE_FRAME;
    vaddr &= PAGE_FRAME;
    ...
    if (as->as_vbase1 == 0) {
        as->as_vbase1 = vaddr; as->as_npages1 = npages; return 0;
    } 
    if (as->as_vbase2 == 0) {
        as->as_vbase2 = vaddr; as->as_npages2 = npages; return 0;
    } 
    kprintf("dumbvm: Warning: too many regions\n");
    return ENOSYS;
}
```
Rispondi SÌ/NO alle seguenti affermazioni/domande:

_Domande:_
* L'istruzione `sz += vaddr & ~(vaddr_t)PAGE_FRAME;` potrebbe essere sostituita dall'istruzione `sz += vaddr % 4096;` **RISPOSTA REPORT PROF CABODI:** YES
* la funzione definisce semplicemente (e memorizza nei campi dati appropriati di una struttura `addrspace`) l'indirizzo
  di partenza e la dimensione di un segmento per ogni chiamata di funzione **RISPOSTA REPORT PROF CABODI:** YES
* L'istruzione `vaddr &= PAGE_FRAME;` cancella i 12 bit meno significativi di un indirizzo fisico. **RISPOSTA REPORT PROF CABODI:** YES
* la funzione alloca memoria fisica per i segmenti di codice e dati, ma non per lo stack **RISPOSTA REPORT PROF CABODI:** NO

---

### Domanda 17

Supponiamo di voler creare una TABELLA DELLE PAGINE a livello di singolo processo. Date le ridotte dimensioni della
RAM fisica, si supponga che una PT entry occupi solo 2 byte. Supponiamo di voler gestire uno spazio di indirizzamento
composto da:
* Segmento 1 (codice): 4 pagine, a partire dall'indirizzo logico $0x400000$
* Segmento 2 (dati): 3 pagine, a partire dall'indirizzo logico $0x500000$
* Stack: 18 pagine, che terminano all'indirizzo logico $0x80000000$ (escluso)

Calcolare la dimensione complessiva di una TABELLA DELLE PAGINE standard.
Esprimere il risultato in termini di B, KB, o MB (es. 10 B, 20 KB, 7 MB)

**RISPOSTA REPORT PROF CABODI:** 1 MB

### Domanda 18

Calcolare quale sarebbe la dimensione di una tabella di pagine organizzata in due distinte tabelle di pagine
standard, una solo per i segmenti di codice e dati (che mappi quindi le pagine dall’inizio di segmento 1 alla
fine del segmento 2) e una per lo stack (solo le pagine dello stack). Scrivere un solo numero, con unità di
misura (B, KB o MB) per la somma di entrambe le dimensioni.

**RISPOSTA REPORT PROF CABODI:** 556 B

---
