## Domanda 2

Sia dato un file di dimensione 160MB, in un file system di dimensione complessiva 400GB. Si vogliono confrontare
le possibili organizzazioni di tale file, su file system con allocazione non contigua, secondo gli schemi:
* "linked list allocation"
* "File Allocation Table (FAT)"

Si supponga che i blocchi su disco (sia per i dati che per metadati) abbiano dimensione 8KB, e che i puntatori e
gli indici abbiano dimensione 32 bit.

Si dica, per ognuna delle 2 soluzioni:

A) Quanti blocchi (di dato e/o di metadato) sono necessari per il file (attenzione: Si richiede il conteggio ESATTO,
eventualmente espresso in termini di potenze di 2, ricordando che $1K=2^{10}$ e $1M=2^{20}$)?<br>
Nel caso della FAT, si dica quale percentuale di FAT è utilizzata per il file.

**Risposta - A**

Intanto converto i dati in byte.
Dim_file = 160 MB = $160 * 10^6$ = 160 * 1024 * 1024 = 167.772.160 B;<br>
Dim_FS = 400 GB = $400*10^9$ B = $4.294 * 10^{11}$ B<br>
Dim_blocchi = 8 KB = $8 * 10^3$ B = 8192 B<br>
Dim_puntatori = Dim_ind = 32 bit = 4 B

La linked list allocation consiste nell'avere dei blocchi i quali ogni blocco contiene un puntatore al blocco successivo.
Potendo essere sparsi all'interno della memoria, non soffriranno di frammentazione esterna.

La FAT è una variante nella quale all'inizio del volume vi è la tabella indicizzata dal numero dei blocchi che facilità l'allocazione dei nuovi blocchi.
Nei blocchi dati quindi contengono solo dati (nessun puntatore interno).

Dim_utile_per_blocco = Dim_blocco - Dim_puntatore = 8 KB - 4 B = 8188 B<br>
N_Blocchi_necessari = ceiling(dim_file / dim_utile_block) = ceil ( $(167.772.160/8188$ ) = ceil(20.490,00489) = 20.491 blocchi;<br>
Overhead_totale = N_Blocchi * Dim_puntatore = 20491 * 4 = 81964 B circa 80 KB + 44 B;

Per la FAT:
Num_entry_FAT = Num_tot_blocchi_nel_FS = DIM_FS /DIM_BLOCCHI = $4.294 * 10^{11} $ / 8192 = 52.428.800 entries; <br>
DIM_FAT = N_entry_FAT * Dim_puntatore = 52.428.800 * 4 = 209.715.200 B<br>
N_blocchi_necessari = ceiling(Dim_file / Dim_blocco) = ceiling(167.772.160) = 20.480 blocchi<br>
Percentuale FAT = (DIM_FAT / DIM_FS) * 100 = $(209.715.200 / 4.294 * 10^{11}) * 100$ = 0,0488 %


B) Quante letture in RAM e quanti accessi a disco (per trasferire un blocco in RAM) sono necessari
per leggere il byte n. 42070 all'interno del file? Si consideri un accesso diretto a file.
Si supponga di non utilizzare buffer cache, e che un puntatore o indice venga letto in RAM con una sola lettura.
Si supponga poi che il File Control Block (FCB) sia già in memoria RAM, che ogni accesso a disco legga o scriva un
blocco di 8KB, e che la FAT (qualora utilizzata) sia già in RAM.

**Risposta - B**

Numero_blocco = floor(Pos_byte / Dim_blocco) = floor(42070 / 8196) = floor(5.13) = 5 (che sarebbe il sesto visto che parte da 0)
Offset_nel_blocco = Pos_byte % Dim_blocco = 1090

Linked List:
Letture_RAM = N_blocchi_attraversare = 5 
Accessi_disco = n_blocchi + 1 = 6

Fat:
Letture RAM = 1 (consultazione FAT)
Accessi_disco = 1 (solo il blocco target)