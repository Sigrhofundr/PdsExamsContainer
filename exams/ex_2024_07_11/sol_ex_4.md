## Domanda 4: Sistema oS161 e Gestione dell'Indirizzamento

Un sistema oS161 è dato, in esecuzione su un simulatore MIPS sys161 con 8MB di RAM. È noto che il processo P1 ha uno spazio di indirizzamento con `as->as_pbase1`, `as->as_pbase2`, `as->as_vbase1`, `as->as_vbase2`, 
`as->as_npages1`, `as->as_npages2` con i seguenti valori: `0x200000`, `0x100000`, `0x4000`, `0x8000`, 3, 4.

Per ciascuno dei seguenti indirizzi fisici, convertire in un indirizzo logico del kernel. Nel caso in cui l'indirizzo sia allocato a P1, convertirlo in un indirizzo logico utente:
* `0x100A00`
* `0x10A0F0`
* `0x206500`
* `0x202D00`

La funzione oS161 `load_elf` imposta lo spazio di indirizzamento di un nuovo processo utente. In diverse fasi, `load_elf` chiama `load_segment`, `as_prepare_load`, `as_complete_load`, `as_define_region`. Rispondere alle seguenti domande:
* Quale funzione viene chiamata per allocare lo spazio di indirizzamento nella memoria fisica? **R** - `as_prepare_load`
* Quale funzione viene chiamata per impostare gli indirizzi logici e le dimensioni dei segmenti di codice e dati? **R** - `as_define_region`
* Quale funzione legge il segmento dati dal file elf? **R** - `load_segment`
* Quale funzione legge il segmento codice dal file elf? **R** - `load_segment`

---

**Risposte**

Avendo indicato che la RAM è pari a 4MB, possiamo dire che:
* 8MB = 8 * 1024 * 1024 = $8.388.608_{10}$ = $800000_{16}$
* Max Kernel Address Ram = $0x80000000 + 800000$ = $0x80800000$
* Assumiamo che le pagine abbiamo dimensioni 4KB = 4096

Per ciascuno dei seguenti indirizzi fisici, convertire in un indirizzo logico del kernel. Nel caso in cui l'indirizzo sia allocato a P1, convertirlo in un indirizzo logico utente:<br>
a. `0x100A00` => K: $0x80100A00$; U: $0x8A00$ <br>
b. `0x10A0F0` => K: $0x8010A0F0$; U: NO<br>
c. `0x206500` => K: $0x80206500$; U: NO<br>
d. `0x202D00` => K: $0x80202D00$; U: $0x6D00$

a. kernel: 100A00 + KSEG0 = 100A00 + 80000000 = 0x80100A00<br>
a. utente: verifichiamo as_pbaseX <= indirizzo_fisico < as_pbaseX + as_npagesX * PAGE_SIZE<br>
a. 100000 <= 100A00 < 100000 + (4 * 4096) --> 100000 <= 100A00 < 100000 + 4000 --> 100000 <= 100A00 < 104000 --> ok<br>
a. offset = ph_addr - ph_base2 = 100A00 - 100000 = A00<br>
a. va = as_vbase2 + offset = 8000 + A00 = 8A00<br>
b. kernel: 10A0F0 + 80000000 = 8010A0F0<br>
b. utente: No, avendo già trovato che il limite superiore è 103000, possiamo dire che essendo l'indirizzo indicato superiore non avrà un indirizzo logico utente<br>
c. kernel: 206500 + 80000000 = 80206500<br>
c. utente: 200000 <= 206500 < 203000 NO, spazio non valido<br>
d. kernel: 202D00 + 80000000 = 80202D00<br>
d. utente: 200000 <= 202D00 < 203000 - OK<br>
d. offset = 202D00 - 200000 = 2D00<br>
d. va = 4000 + 2D00 = 6D00<br>