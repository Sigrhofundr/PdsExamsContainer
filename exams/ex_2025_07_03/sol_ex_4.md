## Domanda 4 (comprende le domande da 15 a 18 del questionario)

### Domanda 15 - Risposte con Sì/No alle affermazioni/domande

_Premessa:_
Si consideri, in OS161, l'implementazione della gestione della memoria virtuale.

A) Supponiamo di voler fare in dumbvm la gestione dei frame liberi tramite free-list, invece che con bitmap
(come fatto nella soluzione proposta per il lab 2)

_Domande:_
* La free list dovrebbe essere utilizzata solo per il segmento di codice e il segmento di dati di un processo, non per
  lo stack utente, poiché lo stack ha una dimensione fissa in dumbvm
* occorre supportare la ricerca di intervalli di frame liberi contigui
* non è necessaria la ricerca di frame liberi contigui, perché MIPS ha una TLB
* La free-list può essere organizzata efficacemente come una lista di intervalli contigui di frame liberi

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
* L'istruzione `sz += vaddr & ~(vaddr_t)PAGE_FRAME;` potrebbe essere sostituita dall'istruzione `sz += vaddr % 4096;`
* la funzione definisce semplicemente (e memorizza nei campi dati appropriati di una struttura `addrspace`) l'indirizzo
  di partenza e la dimensione di un segmento per ogni chiamata di funzione
* L'istruzione `vaddr &= PAGE_FRAME;` cancella i 12 bit meno significativi di un indirizzo fisico.
* la funzione alloca memoria fisica per i segmenti di codice e dati, ma non per lo stack

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

**Risposta**:

---
