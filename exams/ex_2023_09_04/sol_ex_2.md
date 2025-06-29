### Domanda 2

Considera un file system di tipo Unix, basato su *inode*, con 15 puntatori/indici (12 diretti, 1 singolo indiretto, 1 doppio indiretto e 1 triplo indiretto).
I puntatori/indici hanno dimensione di 32 bit e i blocchi disco hanno dimensione di 4KB.<br>
Il file system risiede su una partizione disco, dove 2TB sono riservati ai blocchi dati.
Lo spazio extra riservato ai metadati (inclusi i blocchi indice) può essere trascurato ai fini di questo esercizio.

A) È noto che il file più piccolo nel FS ha dimensione di 8MB e il file più grande ha dimensione di 8GB.
Utilizziamo N2 per il numero di file con indicizzazione doppia e N3 per il numero di file con indicizzazione tripla.
Calcola i valori massimi (i limiti superiori) per N2 e N3.

B) Dato un file binario di dimensione 20490.5KB, calcola esattamente quanti blocchi indice e blocchi dati sta utilizzando il file. <br>
Calcola anche la frammentazione interna (per i blocchi dati).<br>

C) Considera lo stesso file della Domanda B, dove l'operazione `lseek(fd, offset, SEEK_END)` viene chiamata per posizionare
l'offset del file (*SEEK_END* significa che l'offset si riferisce alla fine del file) per la successiva operazione di lettura/scrittura.<br>
Supponi che `fd` sia il *file descriptor* associato al file (già aperto) e che `offset = -2^20`. Calcola il numero di blocco logico
(relativo ai blocchi del file, numerati a partire da 0) al quale la posizione viene spostata, e l'offset interno (all'interno del blocco).

---
**Risposte**

A) Puntatore diretto => (12x4) 48KB di dati;<br>
Indiretto singolo: Dim Blocco / Dim puntatore => 4KB/ 32 bit => 4 * 1024 B / 4 B => 1024 Puntatori => numero per indiretto = n_puntatori * dim_blocchi => 1024 * 4KB = 4 MB<br>
Indiretto doppio: 1024 * 1024 * 4KB = 4GB; <br>
Indiretto triplo: 1024 * 1024 * 1024 * 4KB = 4TB; <br>
Dim Max file: 48 KB + 4 MB + 4 GB + 4 TB = 4.402.319.921.856 Byte (supera la dimensione del disco)<br>

N_blocchi_tot = Dim_Part / Dim_blocchi_disco = 2 TB / 4 KB = 2 * 10^12 / 4 * 10^3 = 0,5 * 10^9 => 512 * 10^6 blocchi<br>

Calcoliamo l'occupazione minima di N2 (tutti i livelli precedenti prima del doppio)<br>
N_2_MIN = 12(diretti) + 1024 (singoli) + 1 (almeno uno doppio) = 1037 blocchi;<br>
calcoliamo l'occupazione minima di N3 (tutti i livelli precedenti prima del triplo)<br>
N_3_MIN = 12 + 1024 + 1024^2 + 1 = 1M + 1037 blocchi dati

Siccome vogliamo calcolare i massimi sia per N2, che per N3, dobbiamo per entrambi i casi porre che ci sia almeno un file
nell'altra categoria di dim minime (quindi 8MB per n3, 8GB per n2):<br>

Vincolo N_2_Max => File 8GB = 8GB/4KB = 2M blocchi<br>
Spazio disponibile per file N2 = 512M - 2M = 510 M blocchi

Vincolo N_3_Max => File 8MB = 8MB/4KB = 2K blocchi<br>
Spazio disponibile per file N3 = 512M - 2k blocchi



N<sub>2_MAX</sub> = floor(spazio_disponibile / Spazio_min_N2) = floor ((512-2M)/2K) = floor(510M / 2K) 255 K file (usa 2K e non 1037 perché il file più piccolo è di 8MB = 2K blocchi)<br>
N<sub>3_MAX</sub> = floor(spazio_disponibile / Spazio_min_N3) = floor ((512M-2K)/(1M + 1037)) = 511 file <br>

B) Convertiamo il file in B:<br>
Dim_file = 20490.5 KB = 20490.5 * 1024 = 20.982.272 B; <br>
Num blocchi necessari = Dim_file/Dim_blocchi_disco = ceil(20.982.272/ 4096) = ceil(5122.625) = 5123;<br>
Con i blocchi diretti e indiretto singolo abbiamo 1036 puntatori, quindi ne rimarranno 5123 - 1036 = 4087 doppi con indiretto doppio.<br>
Indiretto doppio: 4087 => 4087 * 4 KB = 16.740.352 B occupati nell'indiretto doppio;<br>
N_blocchi_indice_doppi = num_blocchi/num_puntatori_blocco = ceil(4087 / 1024) = ceil(3.99) = 4 blocchi indice indiretto doppio<br>
N_blocchi_indice_tot = 1 +1 +4 = 6 blocchi indice<br>
Frammentazione interna = Spazio allocato - dim_file = (5123 * 4 KB) - 20.982.272 B = 1536 B

C) La posizione assoluta sarà data da: <br>
Pos_ass = Dim_file + offset<br>
Offset = -2^20 = -1.048.576 B; <br>
Poss_ass = 20.490.5 KB - 2^20 = 20.982.272 + -1.048.576 = 19933696 B;<br>

Blocco_logico = floor(Pos_assoluta/dim_blocco) = floor(19933696/4096) = floor(4866.625) = 4866; <br>
Offset_interno = Pos_ass % Dim_blocco = 19933696%4096 = 19933696 - 4096 * floor(19933696/4096) = 19933696 - 4096 * 4866 = 2560 B = 2.5 KB