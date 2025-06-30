### Domanda 1

A) Si considerino le affermazioni che seguono, a proposito di possibili vantaggi e svantaggi di una inverted page table (IPT),
rispetto a una tabella delle pagine standard(PT). **Si dica di ognuna se sia vera o falsa, motivando la risposta**

**Vantaggi** L'IPT permette di risparmiare memoria:
1. Si risparmia sempre memoria;
2. Dipende dalle dimensioni della RAM, dal numero di processi e dal loro spazio di indirizzamento virtuale;
3. Si risparmia sempre memoria quando lo spazio di indirizzamento di un processo è maggiore della dimensione della RAM;
4. Si può risparmiare anche in casi in cui lo spazio di indirizzamento di ogni processo è inferiore alla dimensione della RAM;

**Svantaggi**: L’IPT è lenta, perché non garantisce accesso diretto ma occorre una ricerca
1. La chiave di ricerca è il frame;
2. La chiave di ricerca è la pagina;
3. La chiave di ricerca è la coppia (pid, frame);
4. La chiave di ricerca è la coppia (pid, pagina);
5. Per migliorare le prestazioni si sostituisce la IPT con una tabella di HASH;
6. Per migliorare le prestazioni si aggiunge alla IPT una tabella di HASH;

B) Sia dato un processo avente spazio di indirizzamento virtuale di 48 GB, in un sistema dotato di 16GB di RAM, con architettura a 64 bit
(in cui si indirizza il Byte) e gestione della memoria paginata (pagine/frame da 4KB) per i soli processi user, quindi non per il kernel.
Si supponga che 4GB di RAM siano allocati in modo statico al kernel. Si vogliono confrontare una soluzione basata su tabella
delle pagine standard (una tabella per ogni processo) e una basata su IPT. Si calcolino:

B1) Le dimensioni della PT (a un solo livello) per il processo e della IPT. Si ipotizzi che il pid di un processo possa essere rappresentato su 12 bit.
Si utilizzino 28 bit per gli indici di pagina e/o di frame (nella PT o nella IPT) e si tenga conto che, per allineamento, una cella
di IPT o PT può solo essere di 32 o 64 bit.

B2) Si dica infine, utilizzando la IPT proposta (12 bit di pid, 28 bit per un indice di pagina/frame), quale è la dimensione
massima possibile per lo spazio di indirizzamento virtuale di un processo.

---

**Risposte**

A)

**Vantaggi** L'IPT permette di risparmiare memoria:

1. Si risparmia sempre memoria; **Falso** - Non si risparmia sempre: se ci sono pochi processi con piccolo spazio di indirizzamento, la somma delle PT standard può essere più piccola dell’IPT.
2. Dipende dalle dimensioni della RAM, dal numero di processi e dal loro spazio di indirizzamento virtuale; **Vero** -  Il risparmio di memoria dipende da quanti processi ci sono, 
da quanto sono grandi i loro address space e dalla RAM fisica disponibile (che determina la grandezza della IPT). 
3. Si risparmia sempre memoria quando lo spazio di indirizzamento di un processo è maggiore della dimensione della RAM;**Vero** - (con riserva). 
In questi casi, il numero di righe di una PT standard (una per ogni pagina virtuale di ogni processo) è molto maggiore di quello 
della IPT (una riga per ogni frame fisico). Nota: ogni entry IPT è però più grande (deve memorizzare anche il pid), ma il risparmio sul numero di righe è prevalente.
4. Si può risparmiare anche in casi in cui lo spazio di indirizzamento di ogni processo è inferiore alla dimensione della RAM; **Vero** - Perché la IPT è unica per tutti i processi, mentre ogni processo avrebbe comunque una PT separata.

**Svantaggi**: L’IPT è lenta, perché non garantisce accesso diretto ma occorre una ricerca

1. La chiave di ricerca è il frame; **Falso** - Il frame è l’indice nella IPT, non la chiave di ricerca per tradurre indirizzi virtuali.
2. La chiave di ricerca è la pagina; **Falso** - (parzialmente vero) Solo il numero di pagina non basta: serve anche il pid, 
altrimenti più processi con lo stesso numero di pagina entrerebbero in conflitto. La chiave vera è (pid, pagina).
3. La chiave di ricerca è la coppia (pid, frame); **Falso** - Il frame è il risultato della traduzione, non parte della chiave di ricerca.
4. La chiave di ricerca è la coppia (pid, pagina); **Vero** - È questa la chiave di ricerca per trovare se una pagina virtuale di un certo processo è mappata e dove.
5. Per migliorare le prestazioni si sostituisce la IPT con una tabella di HASH; **Falso** - Sostituendo la IPT con una tabella hash si ottiene una soluzione diversa, non una ottimizzazione dell’IPT.
6. Per migliorare le prestazioni si aggiunge alla IPT una tabella di HASH; **Vero** - Di solito si aggiunge una hash table per velocizzare la ricerca della entry (pid, pagina) nella IPT.

<!-- prima versione
**Vantaggi** L'IPT permette di risparmiare memoria:
1. Si risparmia sempre memoria; **Falso** - per quanto, per costruzione venga ridotta la dimensione che avrebbe una tabella delle pagine tradizionale, in cui 
ogni processo possiede la propria PT, nella IPT "base", potrebbero peggiorare le prestazioni in quanto, per ricercare per indirizzo virtuale, essendo le IPT ordinate per 
indirizzo fisico, potrebbe essere necessario scorrere tutta la tabella per trovare l'indirizzo virtuale desiderato. Un miglioramento è comunque dato dall'utilizzo di tabelle 
con hash, ltre che dall'utilizzo del TLB per memorizzare le pagine più utilizzate. 
Un altro punto che potrebbe aumentare la memoria consumata è data dal fatto che, per costruzione, le IPT non possono avere degli indirizzi virtuali condivisi, pertanto 
i riferimenti agli elementi condivisi tra processi non possono essere gestiti a questo livello. Il vero risparmio si ha quando vi sono molti processi aventi un grande spazio di indirizzamento. 
2. Dipende dalle dimensioni della RAM, dal numero di processi e dal loro spazio di indirizzamento virtuale; **Vero** - Per quanto dimensionato nella risposta precedente.
3. Si risparmia sempre memoria quando lo spazio di indirizzamento di un processo è maggiore della dimensione della RAM; **Vero** - Si, è il caso generale per cui è più conveniente il suo utilizzo, grazie al modo 
in cui sono gestiti gli indirizzi, dato che ogni entry nella tabella ha come chiave la coppia data da id-processo e numero di pagina.
4. Si può risparmiare anche in casi in cui lo spazio di indirizzamento di ogni processo è inferiore alla dimensione della RAM; **Vero** - Perchè la IPT è unica per tutti i processi.

**Svantaggi**: L’IPT è lenta, perché non garantisce accesso diretto ma occorre una ricerca
1. La chiave di ricerca è il frame; **Falso** - l'indirizzo fisico viene costruito dal pid e dal numero di pagina virtuale, quindi con questi dati si accederà alla IPT e potrà essere generato, con l'offset trovato, l'indirizzo fisico;
2. La chiave di ricerca è la pagina; **Falso** - la chiave vera è riportata dopo //nota: il prof qui ha messo vero specificando che deve essere in coppia col pid, ma essendoci la relativa risposta dopo secondo me meglio mettere falso;
3. La chiave di ricerca è la coppia (pid, frame); **Falso** - come detto il frame rappresenta l'indice nella tabella;
4. La chiave di ricerca è la coppia (pid, pagina); **Vero** - come riportato più volte. Il pid assume il ruolo di identificatore dello spazio d'indirizzi.
5. Per migliorare le prestazioni si sostituisce la IPT con una tabella di HASH; **Falso** - anche se a seconda dei casi d'uso, potrebbe essere una soluzione ottimale (solitamente per indirizzi superiori ai 32 bit).
6. Per migliorare le prestazioni si aggiunge alla IPT una tabella di HASH; **Vero** - in questo modo si smorzano gli svantaggi indicati in precedenza. In questo modo, la ricerca viene ridotta a un solo, o a pochi, elementi della 
tabella delle pagine. Ogni accesso alla tabella hash aggiunge al procedimento un riferimento alla memoria, quindi un riferimento alla memoria virtuale richiede almeno due letture nella memoria reale: una per 
l'elemento della tabella hash e l'altro per la tabella delle pagine. L'utilizzo della TLB, come accennato, migliorerà le prestazioni.
-->

B) Riepilogo dati:
* spazio_ind_virt = 48 GB;
* RAM = 16 GB;
* arch 64 bit (indirizza il byte)
* Dim_pag-frame = 4 KB solo procesi user
* RAM riservati al kernel = 4GB
* Confronto tra PT standard e IPT

B1)
premesse
- Page Table Standard (PT):
- Una tabella per processo
- Dimensione dipende dallo spazio virtuale del processo
- Entry: contiene frame number + flag di controllo

Inverted Page Table (IPT):
- Una tabella per tutto il sistema
- Dimensione dipende dalla RAM fisica disponibile
- Entry: contiene (PID, virtual page number) + flag

**per PT**

* Numero_pagine_virtuali = Spazio_virtuale_processo / Dimensione_pagina = 48 GB / 4 KB = $48 * 2^{30} / 2^{12}$ = $48*2^{18}$ = 12.582.912 pagine
* Numero_entry_PT = Numero_pagine_virtuali = 12.582.912 entry circa 12M
* Dimensione_singola_entry_PT = per il testo servono 28 bit per gli indici, per allineamento si arriva a 32bit -> 4 byte
* Dimensione_totale_PT = Numero_entry_PT × Dimensione_singola_entry_PT = (circa) 12M * 4 = 48 MB

**per IPT**
* RAM_disponibile_processi = RAM_totale - RAM_kernel = 16-4 = 12GB
* Numero_frame_fisici = RAM_disponibile_processi / Dimensione_pagina = 12 GB / 4 KB = $12 * 2^{30} / 2^{12}$ = $12*2^{18}$ =3.145.728 pagine
* Numero_entry_IPT = Numero_frame_fisici = 3.145.728 = 3 M
* Dimensione_singola_entry_IPT:
    - ci viene detto che il pid ha bisogno di 12 bit
    - 28 bit per l'indice
    - bit richiesti = 28 + 12 + 4 (valid, dirty,ecc) = 44 bit -> dobbiamo scegliere tra 32 e 64, quindi 64 bit -> 8 Byte
* Dimensione_totale_IPT = Numero_entry_IPT × Dimensione_singola_entry_IPT = 3M * 8 = 24 MB

B2) dati i 28 bit disponibili:
* N_max_pag_virtuali = $2^{28}$
* spazio_virt_max = N_max_pag_virtuali * Dim_pagina = $2^{28} * 2^{12}$ = $2^{40}$ = $1.099 * 10^{12}$ circa 1,1TB

L'IPT (24 MB) risulta più efficiente della PT (48 MB) in questo scenario, risparmiando il 50% della memoria per le tabelle di traduzione.
