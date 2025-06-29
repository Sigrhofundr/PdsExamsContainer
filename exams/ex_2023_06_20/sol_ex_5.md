## Domanda 5

**IMPORTANTE:** SE I RISULTATI SONO NUMERI, RIPORTARE PASSAGGI INTERMEDI RILEVANTI E/O FORMULE USATE. LE RISPOSTE SI/NO VANNO MOTIVATE.

Si consideri, in OS161, la realizzazione della gestione della memoria virtuale. Si risponda alle seguenti domande.

A) Si supponga di voler realizzare in `dumbvm` la gestione dei frame liberi mediante free-list, invece che con bitmap (come fatto nella soluzione proposta per il lab 2).<br>
A1) Si dica se la free list può essere una lista di frame oppure una lista di intervalli contigui di frame liberi. (motivare)<br>
A2) Qualora la lista fosse ordinata, andrebbe ordinata in base a indirizzi fisici, logici oppure in base alla dimensione di un intervallo libero?  (motivare)

B) Si supponga di voler invece realizzare una PAGE TABLE a livello di singolo processo. Viste le dimensioni ridotte della RAM fisica, si ipotizzi che una casella (entry) della PT occupi solo 2 Byte. 
Si ipotizzi di NON voler gestire un heap per il processo: il processo contiene quindi unicamente (come fatto in `dumbvm`) i segmenti di codice, di dati e lo stack. Si sa che:
* Il segmento 1 (codice) richiede 4 pagine, inizianti all’indirizzo `0x400000`
* Il segmento 2 (dati) richiede 3 pagine, inizianti all’indirizzo `0x500000`
* Lo stack ha dimensione 18 pagine, che terminano, anziché al massimo indirizzo logico possibile (come fatto in `dumbvm`), all’indirizzo `0x3FFFFFFF`.

B1) Si calcoli la dimensione complessiva dell’address space e, al suo interno, il numero di pagine effettivamente usate (valide).<br>
B2) Si calcoli quale sarebbe la dimensione di una page table organizzata mediante due page table standard distinte, una per i soli segmenti di codice e dato e una per lo stack.

---

**Risposte**

A1) UEntrambe le implementazioni sono possibili, ma hanno pro e contro.
Una lista di frame singoli è più semplice da gestire e permette una rapida allocazione/deallocazione di singole pagine (se la lista non è ordinata, la complessità è O(1)).
Una lista di intervalli contigui è più efficiente in termini di overhead di memoria se la memoria è poco frammentata, in quanto un singolo nodo può rappresentare un grande blocco libero. 
È particolarmente utile quando si devono allocare più pagine contigue, come nel caso delle page table, che richiedono un blocco di memoria contiguo per essere allocate

A2) Essendo una lista per la gestione dei frame fisici, non avrebbe senso ordinarla in base agli indirizzi logici. Se la raggruppassimo per indirizzi fisici ciò 
faciliterebbe la coalescenza dei frame adiacenti; se invece decidessimo l'ordine in base all'intervallo ciò potrebbe risultare utile per utilizzare gli algoritmi
first-fit, best-fit o worst-fit.

B1) 
Riepiloghiamo i dati forniti: <br>
* entry PT = 2B;
* vstack end = `0x3FFFFFFF` (cioè KSEG0/2)
* stack utente = 18 pagine;
* `as->as_vbase1` = `0x400000`, `as->as_npages1` = 4, 
* `as->as_vbase2` = `0x500000`, `as->as_npages2` = 3,

Quindi avremo
* as_stackvbase = vstack_end - (stack_utente * page_size) + 1 = 3FFFFFFF - (18 * 4096)_dec + 1 = 3FFFFFFF - 12000 + 1 = 3FFEE000
* as_stackvbase = $0x3FFEE000$

Il limite inferiore sarà dato da min(as_vbase1,as_vbase2,as_stacvbase) = min(400000,500000,3FFEE000) = as_vbase1 = 0x400000
* Pagine totali = (ind_max - ind_min) / page_size = (1.073.741.823 - 4.194.304 + 1)/4096 =  261120
* Pagine valide = as_npages1 + as_npages2 + stack_pages = 25

B2) Dim_PT = N_pag * entry_size = 261120 * 2 = 522240 B = 510 KB (questa non va bene perché sarebbe quella complessiva)

PT1 - Codice e dati:
* Fine_seg 2 = as_vbase2 + (as_npages2 * Dim_pagina) = 500000 + (3 * 1000) = $0x503000$
* Range_PT1 = Fine_seg2 - as_vbase1 = 503000 - 400000 = $0x103000$
* Pagine_pt1 = range_pt1 / dim_pag = (103000/ 3000)_ex = 1.060.864/4096 = 259 pag
* Dim_Pt1 = n_pag_pt1 * dim_entry = 259 * 2 = 518 B

PT2 - stack:
* Range_pt2 = vstack_end - as_stackvbase = 3FFFFFFF - 3FFEE000 = $0x11FFF$
* Pagine_pt2 = 11FFF/1000 = 73727+1/4096 = 18
* Dim_pt2 = n_pag_pt2 * dim_entry = 18 * 2 = 36 B

Dim_complessiva_pt = Dim_pt1 + Dim_pt2 = 554 B