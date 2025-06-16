## Domanda 1

Considerare un file system Unix-like, basato su inode, con 13 puntatori / indici (10 diretti, 1 singolo
indiretto, 1 doppio indiretto e 1 triplo indiretto). I puntatori / indici hanno una dimensione di 32 bit e i
blocchi del disco hanno una dimensione di 2 KB. Il file system risiede su una partizione del disco di 800
GB, che _include sia blocchi di dati che blocchi di indice_.

A) Supponendo che tutti i metadati (eccetto i blocchi indice) abbiano dimensioni trascurabili,
calcolare il numero massimo di file che il file system puo ospitare, utilizzando l'indicizzazione
indiretta doppia (N2) e l'indicizzazione indiretta tripla (N3).

**Risposta**
Riepilogando i dati:<br>
Dim_puntatori = 32 bit = 4 B;
Dim_blocchi_dati = 2 KB; // approssiamo, non moltiplichiamo 2 * 1024

N_Blocchi_Max = Spazio_tot / Dim_blocchi = 800 GB / 2 KB = $400*10^9 / 10^3 $ = 400 * 10^6 = 400 M

$Dati_{PuntatoreDiretto}$ = 10 * 2 KB = 20 KB<br>
Indiretto singolo = Dim_blocco / Dim_puntatore = 2 KB / 4 = 512 puntatori<br>
Dim_indiretto => n_puntatori * dim_blocchi = 512 * 2 KB = 1024 KB circa 1 MB<br>
Indiretto doppio = 512 * 512 * 2KB circa 524.288.000 circa 512 MB<br>
Indiretto triplo = 512 * 512 * 512 * 2KB circa 2.74 * 10^11 circa 274 GB<br>

Mi calcolo prima l'occupazione minima
$N_{2 MIN}$ = (10(diretti) + 512 (singoli) + 1 (almeno uno doppio))dati + 1(indice singolo) + 2(doppio) blocchi indice = 526 blocchi

calcoliamo l'occ minima di N3 (tutti i livelli precedenti prima del triplo)
$N_{3 MIN}$ = (10(diretti) + 512 (singoli) + $512^2$ + 1)dati  1 (singolo) + 1 + 512 (doppio) + 1 + 1 + 1 (triplo) blocchi indice= 262.144 + 523 + 517 = 263.184 blocchi

$N_{2MAX}$= floor(N_Max_blocchi / N_2_min) = floor (400 M / 526) = floor(760456.27) = 760456 = 0,76 M
$N_{3MAX}$= floor(N_Max_blocchi / N_3_min) = floor (400 M / 263.184) = floor (1519.84) = 1519


B) Dato un file binario di dimensione 10240.5KB, calcolare esattamente quanti blocchi indice e
blocchi di data occupa il file.'

**Risposta**

Dim_file = 10.240,5 KB = 10.486.272 B
N_blocchi_necessari = Dim_file / Dim_blocchi_disco = ceil(10.486.272/ 2048) = ceil(5120.25) = 5121<br>
Dai livelli precedenti dell'indiretto doppio abbiamo 525 blocchi => 5121 - 525 = 4596 da indiretto doppio<br>
Per ricavarmi i blocchi indice:<br>
N_blocchi_indice_doppi = ceil(num_blocchi/num_puntatori_blocco) = ceil (4596/512) = ceil (8.97) = 9 blocchi indice indiretto doppio
N_indice_tot = 1 + 1 + 9 = 11 blocchi

C) Considerare lo stesso file della domanda B, su cui viene chiamata l'operazione lseek (fd, offset,
SEEK_END) per posizionare l'offset del file per la successiva operazione di lettura / scrittura
(SEEK_END significa che l'offset viene calcolato dalla fine del file). Supponiamo che fd sia il
descrittore di file associato al file (gia aperto), e offset= $0x00800000$. Calcolare il numero di
blocco logico (numerazione relativa ai blocchi del file, numerati a partire da 0) in corrispondenza
del quale viene spostata la posizione (da lseek). Se il file utilizza un'indicizzazione indiretta singola
(o doppia / tripla), calcolare quale riga del blocco di indici esterno contiene l'indice del blocco di
dati (o l'indice del blocco di indici piu interno).

**Risposta**

Calcolo pos finale:

<!--Sol prof-->
l'offset viene posto a 10240.5K-Ox00800000 = 0x00A00200-0x00800000 = 0x00200200
lndice del blocco dati: 0x00200200/2K = (2M+515)/2K = 1 K = 0x400
10 blocchi di dati sono diretti
512 blocchi di dati sono a singoli indiretti
II blocco e al doppio livello indiretto
lndici esterni = (1K-512-10)/512 = 0

<!-- Con i miei calcoli
Pos_finale = Dim_file + offset<br>
Fine_file = 10240.5 K <br>
Offset = $0x00800000$ => $8388608_{10}$ <br>
Pos_finale = 18.874.880

N_blocco_logico = floor(pos_finale/dim_blocco) = floor(9216.25) = 9216<br>
questo è compreso tra 512 e 512^2 => indiretto doppio

Blocco relativo = blocco logico - 522 = 8694;<br>
Indice_esterno = floor(blocco_relativo/512) = floor(16.98) = 16
indice_interno = blocco_relativo % 512 = 502
-->
