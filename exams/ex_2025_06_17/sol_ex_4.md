
## Domanda 4 (comprende le domande da 12 a 13 del questionario)

Si consideri un sistema oS161, in esecuzione su un simulatore MIPS sys161 con 16 MB di RAM.  È noto che il processo P1 ha uno spazio di indirizzamento con `as->as_pbasel`, `as->as_pbase2`, `as->as_vbasel`, `as->as_vbase2`, `as->as_npages1`, `as->as_npages2`, `as->as_stackpbase` con i seguenti valori: `0x300000`, `0x100000`, `0x4000`, `0x8000`, 3, 4, `0x200000`.  `PAGE_SIZE` è 4096 e `DUMBVM_STACKPAGES` è 18, e la definizione del tipo è

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
----

### Domanda 12

**Punteggio max.: 2,00**

Per ciascuno dei seguenti indirizzi fisici, convertirlo in un indirizzo logico kernel. Nel caso in cui l'indirizzo sia
allocato a P1, convertirlo in un indirizzo logico user:
* `0x100A00`
* `0x20A0F0`
* `0x21A500`
* `0x302D00`

* `0x100A00` - kernel
* `0x20A0F0` - kernel
* `0x21A500` - kernel
* `0x302D00` - kernel
* `0x100A00` - user
* `0x20A0F0` - user
* `0x21A500` - user
* `0x302D00` - user
----

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