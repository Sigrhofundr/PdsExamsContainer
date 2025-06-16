## Domanda 2: File System basato su Inode

Considerare un file system di tipo Unix basato su inode, con 15 puntatori (12 diretti, 1 indiretto singolo, 1 indiretto doppio e 1 indiretto triplo). I puntatori hanno una dimensione di 32 bit e i blocchi disco hanno una dimensione di 4 KB. Il file system risiede su una partizione
disco dove 1 TB è riservato per i blocchi dati. Lo spazio extra riservato per i metadati (inclusi i blocchi indice) può essere trascurato ai fini di questo esercizio.
<br>
A) **D** - Calcolare la dimensione massima del file supportata da questo file system.<br>
**R** : <br>
Puntatore diretto => (12x4) 48KB di dati;<br>
Indiretto singolo: Dim Blocco / Dim puntatore => 4KB/ 32 bit => 4 * 1024 B / 4 B => 1024 Puntatori => numero per indiretto = n_puntatori * dim_blocchi => 1024 * 4KB = 4 MB<br>
Indiretto doppio: 1024 * 1024 * 4KB = 4GB; <br>
Indiretto triplo: 1024 * 1024 * 1024 * 4KB = 4TB; <br>
Dim Max file: 48 KB + 4 MB + 4 GB + 4 TB = 4.402.319.921.856 Byte (supera la dimensione del disco)<br>
B) **D** - Dato un file binario di dimensione 25.600 KB, calcolare esattamente quanti blocchi indice e blocchi dati sta utilizzando il file. Calcolare anche la frammentazione interna (per i blocchi dati).<br>
**R** - Ci serviranno 14 puntatori (escludiamo il triplo) e stiamo escludendo i metadati: <br>
Num blocchi necessari = Dim_file/Dim_blocchi_disco = (25600 * 1024) / (4 * 1024) = 25600/4 = 6400 blocchi<br>
Sommando i blocchi del puntatore diretto e dell'indiretto singolo otteniamo => 1024 + 12 => 1036 blocchi => 4MB + 48KB = 4.243.456 B<br>
Blocchi rimanenti => 6400 - 1036 = 5364 blocchi negli indiretti doppi <br>
Indiretto doppio: 5364 => 5364 * 4 KB = 21456KB = 21.970.944 B ; inoltre dal numero dei puntatori ci ricaviamo il numero di blocchi indice => 5364/1024 = 5.23 => 6 blocchi indice<br>
Questi sommati ai 2 (1+1) dei livelli precedenti, TOT BLOCCHI INDICE = 1 + 1 + 6 = 8; <br>
File originale = 25600 KB = 25600 * 1024 = 26.214.400 B <br>
La somma dei bytes totali viene senza delta => nessuna frammentazione interna per le ipotesi poste.<br>
C) **D** - Supponiamo che venga chiamata un'operazione `lseek(fd, offset, SEEK_END)` per posizionare l'offset del file per una successiva operazione di lettura/scrittura.
Dato un file binario di dimensione 10 GB e un offset di -1.048.576 byte (1 MB) dalla fine del file, calcolare il numero di blocco logico (relativo ai blocchi del file, numerati a partire da 0)
a cui la posizione viene spostata.<br>
**R** - Intanto convertiamo i dati: <br>
Dimensione file = 10 GB = 10 * 1024 * 1024 * 10124 = 10.737.418.240 B = 2.621.440 blocchi <br>
Offset = -1.048.576 byte = -1.048.576 byte / 4096 = -256 blocchi <br>
Posizione assoluta = DIM_FILE + offset = 10.737.418.240 + (-1.048.576) = 10.736.369.664 <br>
Blocco logico = floor(Posizione_assoluta/Dimensione_blocco) = 10.736.369.664 / 4096 = 2.621.184° blocco <br>
Offset_nel_blocco = Posizione assoluta % dim_blocco = 0