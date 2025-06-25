
## Domanda 4 (comprende le domande da 12 a 13 del questionario)

Si consideri un sistema oS161, in esecuzione su un simulatore MIPS sys161 con 16 MB di RAM.  
È noto che il processo P1 ha uno spazio di indirizzamento con `as->as_pbase1`, `as->as_pbase2`, `as->as_vbase1`, `as->as_vbase2`, `as->as_npages1`, `as->as_npages2`, `as->as_stackpbase` 
con i seguenti valori: `0x300000`, `0x100000`, `0x4000`, `0x8000`, 3, 4, `0x200000`.  `PAGE_SIZE` è 4096 e `DUMBVM_STACKPAGES` è 18, e la definizione del tipo è

```c
struct addrspace {
    vaddr_t as_vbasel;
    paddr_t as_pbasel;
    size_t as_npages1;
    vaddr_t as_vbase2;
    paddr_t as_pbase2;
    size_t as_npages2;
    paddr_t as_stackpbase;
};
```
---

### Domanda 12

**Punteggio max.: 2,00**

Per ciascuno dei seguenti indirizzi fisici, convertirlo in un indirizzo logico kernel. Nel caso in cui l'indirizzo sia
allocato a P1, convertirlo in un indirizzo logico user:
* `0x100A00`
* `0x20A0F0`
* `0x21A500`
* `0x302D00`

**Risposte**

Per quelli kernel sommiamo KSEG0 per ottenere l'indirizzo logico kernel;<br>
Calcoliamo la Dim della RAM (in esadecimale) => 16 MB = 16 * 1024 * 1024 = 16777216_dec B = $01000000$<br>
* 1_ verifichiamo as_pbaseX <= indirizzo_fisico < as_pbaseX + as_npagesX * PAGE_SIZE
* 1_ 100000 <= 100A00 < 100000 + (4_dec*4096_dec)
* 1_ 100000 <= 100A00 < 104000
* 1_ offset = ph_addr - ph_base2 = $100A00 - 100000$ = $A00$
* 1_ virtual address = as_vbase2 + offset = $8000$ + $A00$ = $0x8A00$

* 2_ per l'indirizzo sarà nello stack - as_stackpbase <= indirizzo_fisico < as_stackpbase + DUMBVM_STACKPAGES * PAGE_SIZE)
* 2_ 200000 <= 20A0F0 < 200000 + (18_dec*4096_dec)  --> 200000 <= 20A0F0 < 200000 + 120000 --> 200000 <= 20A0F0 <  320000
* 2_ offset = ph_addr - ph_basestack = $20A0F0 - 200000$ = $A0F0$
* 2_ lo stack parte virtualmente da USERSTACK - DUMBVM_STACKPAGES*PAGE_SIZE = $80000000 - (DUMBVM STACKPAGES * PAGE_SIZE)$ = $80000000 - 120000$= 7FFEE000
* 2_ va = USERSTACK_result + offset = $7FFEE000 + A0F0$ = $0x7FFF80F0$

* 3_ invalido in quanto non rientra in nessuna delle base
* 4_ 300000 <= 302D00 < 300000 + (3*4096_dec) --> 300000 <= 302D00 < 300000 + 3000 --> 300000 <= 302D00 < 303000 ok
* 4_ va = 4000 + (302D00-300000) = 4000 + 2D00 = 6D00

* `0x100A00` - kernel => $0x80100A00$
* `0x20A0F0` - kernel => $0x8020A0F0$ 
* `0x21A500` - kernel => $0x8021A500$
* `0x302D00` - kernel => $0x80302D00$
* `0x100A00` - user =>  $0x8A00$
* `0x20A0F0` - user =>  $0x7FFF80F0$
* `0x21A500` - user =>  non esiste un indirizzo virtuale user valido per quell’indirizzo fisico
* `0x302D00` - user =>  $0x6D00$
---

### Domanda 13

**Punteggio max.: 1,00**

Sono dati i seguenti prototipi OS161 per funzioni in _dumbvm_
```c
paddr_t getppages(unsigned long npages);
paddr_t ram_stealmem(unsigned long npages);
void free_kpages(vaddr_t addr);
int as_prepare_load(struct addrspace *as);
```

Rispondere SÌ/NO alle seguenti domande:

* la funzione as_prepare_load restituisce la dimensione dello spazio degli indirizzi, espressa come numero di pagine.
* getppages chiama ram_stealmem
* type vaddr_t can be used for kernel and user logical addresses
* il tipo vaddr_t può essere usato per indirizzi logici sia kernel che user
* free_kpages (chiamata da kfree) può essere utilizzata per liberare lo stack di livello kernel di un processo