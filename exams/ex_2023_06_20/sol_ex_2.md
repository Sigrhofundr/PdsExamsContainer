## Domanda 2

**IMPORTANTE:** TUTTE LE RISPOSTE SÌ / NO DEVONO ESSERE MOTIVATE. QUANDO I RISULTATI SONO NUMERI, SONO NECESSARI IL RISULTATO FINALE E I RELATIVI PASSI INTERMEDI (O FORMULE).

Considerare un file system basato su allocazione indicizzata nel quale si utilizza per ogni file una lista di blocchi indice.
ATTENZIONE: non si tratta di organizzazione Unix-UFS basata su INODE, ma solo di lista di blocchi indice.
I puntatori/indici hanno una dimensione di 32 bit e i blocchi del disco hanno una dimensione di 4KB.
Il file system risiede su una partizione del disco in cui 2TB sono riservati a **blocchi di dato e blocchi di indice**,
a cui si aggiungono 2GB riservati a metadati (FCB, direttori, ecc., blocchi indice esclusi in quanto allocati insieme ai blocchi dato).

A) **D** - Calcolare la massima dimensione possibile per un file (si vuole la dimensione esatta del file).<br>
**R** - Dati forniti:<br>
Dim_puntatore = 32 bit = 4B; Dim_blocchi = 4 KB = 4096 B;<br>
Puntatori_per_blocco_indice = Dim_blocco / Dim_Puntatore = 1024 puntatori<br>
Puntatori_dati_per_blocco = Puntatori_per_blocco_indice - 1 = 1023 => Ogni puntatore indirizzerà un blocco<br>
Lo spazio totale - 2 TB - deve essere distribuito tra blocchi dato (x) e blocchi indice (y), quindi avremo: <br>
x + y = spazio totale /dim_blocco ;<br>
x = y * puntatore_dati_per_blocco; <br>
=> dalla seconda eq: (y*punt_dati_per_blocco) + y = spazio totale /dim blocco => (y*1023) + y =  2.199.023.255.552 B / 4096 B => y(1023 +1) = 536.870.912 => y = 536.870.912/1024 => y = 524288 blocchi indice;<br>
=> dalla prima: x + 524.288 = 536.870.912 => x = 536.346.624<br>
Riepilogando:<br>
**Blocchi dati**: 536.346.624
**Blocchi indice**: 524.288
**Dim_max_file** = Numero_blocchi_dati_massimo × Dimensione_blocco = 536.346.624 * 4096 = 2.196875772 * 10^12 TB  (inferiore allo spazio disponibile, il delta sarà dato dallo spazio occupato dai blocchi indice!)<br>
B) **D** - Dato un file binario di dimensione 8245 KB, calcolare quanti blocchi indice e blocchi di dato occupa il file.
Calcolare anche la frammentazione interna, sia per i blocchi di dato che per quelli di indice.<br>
**R** - Convertiamo il file -> Dim_file = 8245 KB = 8245 * 1024 B = 8.442.880 B<br>
N_blocchi_dati = Dim_file /dim_blocco = 8.442.880 / 4096 = 2061.25 => 2062 <br>
Spazio_allocato_dati = n_blocchi_dati * dim_blocco = 2062 * 4096 = 8.445.952 B <br>
Frammentazione_dati = Spazio_allocato_dati - Dim_file = 8.445.952 - 8.442.880 = 3072 B <br>
Blocchi indice = blocchi_dati / puntatori_dati_per_blocco = 2062 / 1023 = 2.015 = 3<br>
Puntatori_usati = Blocchi_dati = 2062; <br>
Puntatori_allocati = blocchi_indice * 1023 = 3 * 1023 = 3069; <br>
Puntatori_sprecati = Puntatori_allocati - puntatori_usati = 3069 - 2062 = 1007; <br>
Frammentazione_indici = Puntatori_sprecati × Dimensione_puntatore = 1007 * 4 = 4028 B ; <br>

C) **D** - Si sa che il file (vedi domanda B) contiene una sequenza di numeri reali in formato double (64 bit per numero).
Si calcoli fino a quale blocco di indice e a quale riga (casella) di questo occorre accedere per leggere i primi 512K (K va inteso come 2^10) numeri del file.
(Verificare che la dimensione sia consistente).<br>
**R** - Numeri = 512K = 512 *2^10 = 512 *1024 = 524288 numeri;<br>
Dim_numero = 64bit = 8 B; <br>
Dim_totale = Numeri * Dim_numero = 524288 * 8 = 4.194.304 B;

Blocchi_dati_necessari = Dim_tot / dim_blocco = 4.194.304 / 4096 = 1024 blocchi<br>
Resto = Dim_totale % dim_blocco = 0<br>
Pos_ultimo_blocco = Resto = 0<br>

Blocco_indice = blocchi_dati_necessari / Puntatori_dati_per_blocco = 1024/1023 = 2<br>
Riga_nel_blocco_indice = 1024%1023 = 1<br>