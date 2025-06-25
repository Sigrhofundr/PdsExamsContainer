## Domanda 4

È dato un sistema OS161, in esecuzione su un simulatore sys161 MIPS con 4MB di memoria RAM. Per ciascuno dei seguenti indirizzi, 
indica se può essere un indirizzo logico utente, un indirizzo logico kernel, un indirizzo fisico (spiega/motiva le risposte):

1. `0x80803005`
2. `0x312010`
3. `0x532100`

Dato un indirizzo logico utente `0x4010`, convertilo nel relativo indirizzo fisico. 
È noto che `as->as_pbase1`, `as->as_pbase2`, `as->as_vbase1`, `as->as_vbase2`, `as->as_npages1`, `as->as_npages2` hanno i seguenti valori:
`0x100000, 0x200000, 0x3000, 0x6000, 2, 4`.

La memoria fisica in `dumbvm` è allocata per multipli di una pagina, nonostante sia uno schema di allocazione contigua, perché:

* l'allocazione per multipli di una pagina riduce la frammentazione interna
* la MMU in MIPS ha una TLB, quindi la traduzione da logico a fisico richiede pagine
* `dumbvm` implementa una tabella delle pagine
* `kmalloc` può allocare solo per multipli di pagine.

---

**Risposte**

Premesse:<br>
In un processore MIPS a 32 bit, lo spazio di indirizzamento è di 4 GB, suddivisi in 2GB per il kernel (parte alta) e 2GB per l'utente (parte bassa).
Quindi gli indirizzi logici utente vanno da $0x00000000$ a $0x7FFFFFFF$, da $0x80000000$ saranno indirizzi kernel
L'indirizzo virtuale del primo spazio libero per l'allocazione è sempre lo stesso: $0x8003b000$ (`firstfree`). A partire da questo indirizzo virtuale si calcola 
l'indirizzo fisico corrispondente (`firstpaddr`), sottraendo una costante chiamata `MIPS_KSEG0`, che è pari a $0x80000000$, e calcolando abbiamo che
`firstpaddr` = $0x3b000$.

Avendo indicato che la RAM è pari a 4MB, possiamo dire che:
* 4MB = 4 * 1024 * 1024 = $4194304_{10}$ = $400000_{16}$
* da sopra, l'indirizzo logico kernel max sarà dato da `Valore RAM + MIPS_KSEG0` = $400000$ + $0x80000000$ = $0x80400000$
* max physical addr. = RAM = $0x400000$

In generale, il kernel può “usare” (vedere, generare, manipolare) indirizzi oltre $0x80400000$, ma se prova ad accedervi come RAM fisica 
ottiene un errore perché la RAM oltre i 4MB non esiste fisicamente. Quindi: lo spazio di indirizzamento esiste, la RAM fisica no!

Per cui:
1. `0x80803005` -> Potrebbe essere un indirizzo logico kernel se la Ram fosse superiore a 8 MB, in questo caso non è un indirizzo valido;
non sarà sicuramente un indirizzo logico utente in quanto è superiore ai 2GB (superiore a $0x80000000$);
2. `0x312010` -> potrebbe rappresentare sia un indirizzo fisico che logico per l'utente, in quanto è un indirizzo fisico superiore a firstpaddr e minore di
`MIPS_KSEG0` per quello logico;
3. `0x532100` -> come nel caso precedente,sarà solo un indirizzo logico utente in quanto superiamo i limiti di Ram nella rappresentazione dell'indirizzo fisico;

---

Premesse:<br>
La dimensione di una pagina solitamente è di 4KB = 4096 B; lo spazio kernel (2GB) è diviso in 3 aree: kseg0 e kseg1 da 512MB ciascuno e kseg2 da 1GB. Nel dettaglio:
* kseg0 - da $0x80000000$ a $0x9FFFFFFF$;
* kseg1 - da $0xA0000000$ a $0xBFFFFFFF$
* kseg2 - da $0xC0000000$ a $0xFFFFFFFF$

I principali campi coinvolti sono:
* as_vbase1 e as_npages1: base e dimensione (in pagine) della prima regione virtuale (tipicamente il segmento di codice/dati).
* as_pbase1: base fisica della prima regione.
* as_vbase2, as_npages2, as_pbase2: analoghi per la seconda regione (tipicamente segmento dati/bss).

#### Procedura generale per la conversione

1. Identifica la regione virtuale corrispondente all’indirizzo logico dato:
   * verifica se l’indirizzo logico fornito (va) rientra in una delle due regioni, usando as_vbase1, as_npages1, as_vbase2, as_npages2.
   * per ciascuna regione: `as_vbaseX <= va < as_vbaseX + as_npagesX * PAGE_SIZE` (dove X = 1 o 2).
2. Calcola l'offset all'interno della regione:
   * Determina di quanti byte l’indirizzo logico fornito si discosta dalla base della regione virtuale: `offset = va - as_vbaseX`
3. Trova la base fisica della regione corrispondente:
   * Prendi il valore di as_pbaseX relativo alla regione trovata.
4. Calcola l’indirizzo fisico:
   * L’indirizzo fisico si ottiene aggiungendo l’offset calcolato alla base fisica: `pa = as_pbaseX + offset`

**Nota**: Se l’indirizzo logico non rientra in nessuna delle due regioni, non è mappato e la conversione non è valida.

Riepilogando:
* Indirizzo logico utente da convertire `0x4010` in fisico
* `as->as_pbase1` = `0x100000`;
* `as->as_vbase1` =  `0x3000`;
* `as->as_npages1` = `2`;
* `as->as_pbase2` = `0x200000`;
* `as->as_vbase2` = `0x6000`;
* `as->as_npages2` = `4`;

as_vbaseX <= 0x4010 < base virtual address del segmento X + (numero pagine del segmentoX * dim_pagina)<br>
sostituendo per il primo<br>
0x3000 <= 0x4010 < 0x3000 + (2* 4096)<br>
0x3000 <= 0x4010 < 0x5000 => è nel range<br>
offset = va - as_vbase1 = 0x4010 - 0x3000 = 0x1010<br>
physical address = as_pbase1 + offset = 0x100000 + 0x1010 = 0x101010

---

La memoria fisica in `dumbvm` è allocata per multipli di una pagina, nonostante sia uno schema di allocazione contigua, perché:

1. l'allocazione per multipli di una pagina riduce la frammentazione interna
2. la MMU in MIPS ha una TLB, quindi la traduzione da logico a fisico richiede pagine
3. `dumbvm` implementa una tabella delle pagine
4. `kmalloc` può allocare solo per multipli di pagine.

**Risposte**

1. No. Allocare per multipli di una pagina può aumentare, non ridurre, la frammentazione interna, in quanto se si dovesse chiedere meno di una pagina il resto verrebbe sprecato.
In allocazione contigua pura (senza padding), non ci sarebbe frammentazione interna.
2. Sì. LA MMU usa la TLB per lavorare a livello di pagina: la traduzione degli indirizzi da logici a fisici avviene su base di pagine, quindi anche l'allocazione 
deve rispettare i confini di pagina.
3. No. La dumbvm è chiamata così anche perché non implementa una vera page table, ma usa uno schema semplice di allocazione contigua.
4. No. kmalloc non ha limiti di allocazione.
