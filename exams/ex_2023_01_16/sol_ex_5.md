### Domanda 5

Si consideri, in OS161, la realizzazione della gestione della memoria virtuale. Si risponda alle seguenti domande:

A) Si supponga di voler realizzare in dumbvm la gestione dei frame liberi mediante free-list, invece che con bitmap
(come fatto nella soluzione proposta per il lab 2).
A1) Si dica se la free list può essere una lista non ordinata oppure se deve essere ordinata. (motivare)
A2) Qualora la lista fosse ordinata, andrebbe ordinata in base a indirizzi fisici oppure logici? (motivare)

B) Si supponga di voler invece realizzare una PAGE TABLE a livello di singolo processo.
Viste le dimensioni ridotte della RAM fisica, si ipotizzi che una casella (entry) della PT occupi solo 2 Byte.
Si ipotizzi di NON voler gestire un heap per il processo: il processo contiene quindi unicamente (come fatto in dumbvm)
i segmenti di codice, di dati e lo stack. Si sa che:
* La costante PAGE_SIZE è definita come 4096.
* Il segmento 1 (codice) inizia all'indirizzo logico 0x400000 e ha dimensione 10112 Byte.
* Il segmento 2 (dati) inizia all'indirizzo logico 0x412000 e ha dimensione 8028 Byte.
* Lo stack ha dimensione 18 pagine, collocate ai massimi indirizzi logici possibili.

B1) Si calcoli la dimensione complessiva dell'address space e, al suo interno, il numero di pagine effettivamente usate (valide).
B2) Si calcoli quale sarebbe la dimensione di una *page table* standard.
B3) Si calcoli quale sarebbe la dimensione di una *page table* gerarchica a due livelli, supponendo di utilizzare 11 bit per p2: si ricorda che l'indirizzo logico è (p1,p2,d).

---
**Risposte**

A1) è uno scenario possibile; la lista, Se non fosse ordinata, avrebbe una complessità di accessi pari a O(1). Tuttavia, una lista di intervalli contigui 
sarebbe più efficiente in termini di overhead di memoria se la memoria fosse poco frammentata, in quanto un singolo nodo può rappresentare un grande blocco libero.
Sarebbe particolarmente utile nel caso in cui si dovessero allocare più pagine contigue, come nel caso delle page table, che richiedono un blocco di memoria contiguo per essere allocate.

A2) Essendo una lista per la gestione dei frame fisici, non avrebbe senso ordinarla in base agli indirizzi logici. Se la raggruppassimo per indirizzi fisici ciò faciliterebbe la coalescenza dei frame adiacenti.

B1) Riepilogando i dati forniti:
- entry PT: 2 Byte;
- vstack end = KSEG0 -1  = $0x7FFFFFFF$;
- stack utente = 18 pagine;
- Dim_page = 4096 B = 1000_ex
- `as->as_vbase1` = $0x400000$; Dim_seg_1 = 10112 Byte;
- `as->as_vbase2` = $0x412000$; Dim_seg_2 = 8028 Byte;

Calcolando:
* as_stackvbase = vstack_end - (stack_utente * page_size) + 1 = 0x7FFFFFFF - (12 * 1000) + 1 =  $0x7FFEE000$;
* lim_inferiore_addr = min(as_vbase1,as_vbase2,as_stacvbase) = min(400000,412000,7FFEE000) = as_vbase1 = $0x400000$
* Pagine_segmento (as->as_npages#) = ceil(Dim_seg / page_size) 
* as->as_npages1 = = ceil(10112/4096) = ceil(2.46875) = 3;
* as->as_npages2 = = ceil(8028/4096) = ceil(1.96) = 2;
* Fine_segmento = Inizio_segmento + Dimensione_bytes - 1
* end_vbase1 = 400000 + 10112 - 1 = 400000 + 2780 - 1 = 402779; 
* end_vbase2 = 412000 + 8028 - 1 = 412000 + 1F5C - 1 = 413F5B; 
* range indirizzi = (ind_max-ind_min+1) = (0x7FFFFFFF-400000+1) = 7FC00000 = 2.143.289.344_dec
* Pagine_tot = (range indirizzi) / page_size = 7FC00000/1000 = 7FC00_ex = 523264 pagine
* Pagine_valide = as_npages1 + as_npages2 + stack_pages = 3 + 2 + 18 = 23 pagine

B2) Dim_PT = N_pag * entry_size = 523264 * 2 = 1.046.528 B = 1022 KB circa 1MB

B3) Calcoliamo:
* Bit_totali = $log_2(spazio indirizzi)$ = $log_2(2.143.289.344)$ circa 31
* bit_offset = $log_2(Page_size)$ = $log_2(4096)$ = 12 bit
* Bit_p1 = bit_totali - bit_p2 - bit_offset = 31 - 11 - 12 = 8 bit
* Entry_primo_livello = $2^{bit p1}$ = $2^8$ = 256;
* PT_primo_liv = entry* dim_entry = 256 * 2 = 512 Byte
* Entry_secondo_liv = $2^{bit p2}$ = $2^11$ = 2048;
* Pt_secondo_liv = 2048 * 2 = 4096 Byte;
* Sapendo che la pt di secondo livello potrebbe contenere 2048 entries, ne basta 1 per contenere 23 pagine.
* Dim_totale_PTs = PT_primo + PT_sec*n_Pt_2_lv = 512 + 1*4096 = 4608 Bytes

**NOTA PERSONALE** Personalmente sono perplesso per il B3 sul procedimento di calcolo, il prof riporta:<br>
Siccome d ha dimensione 12 bit, restano per p1 9 bit. Quindi la _outer_ PT avrà dimensione $2^9 * 2B = 1 KB$, mentre 
una _inner_ PT ha dimensione $2^{11} * 2B = 4 KB$ (un frame). Lo stack è coperto da una solla inner PT per i due segmenti (codice e dati).
Occorre verificare i valori di p1:<br>
$0x00400000$ =  00000000 0,10000000000000000000000 <br>
$0x00412000$ =  00000000 0,10000010010000000000000 <br>
Siccome i due segmenti condividono lo stesso valore di p1, è sufficiente per entrambi la stessa inner PT.
In conclusione, la PT gerarchica occuperà un frame (usato per un quarto) per la outer PT, e due frame per la inner PT.
