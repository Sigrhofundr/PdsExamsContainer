## Domanda 2
Consideriamo un file system basato su allocazione indicizzata in cui, per gestire file di grandi dimensioni, i blocchi
di indice sono organizzati in una lista collegata. I puntatori/indici hanno una dimensione di 32 bit e i blocchi disco hanno una dimensione di 4KB.
Il file system risiede su una partizione disco da 1TB, che include sia blocchi di dati che blocchi di indice.

A - Dato un file binario di dimensioni 23033 KB, calcola esattamente quanti blocchi di indice e blocchi di dati occupa il file.
Calcola anche la frammentazione interna, sia per i blocchi di dati che per i blocchi di indice.

B - Un file di testo B, di dimensioni 15300 B, contiene una sequenza di righe a lunghezza variabile, ciascuna terminata da '\n'.
Sappiamo che la lunghezza media della riga è di 50 caratteri (escluso '\n') e la lunghezza massima della riga è di 100.
Calcola il numero di righe nel file (nel caso in cui il numero vari tra un valore minimo e uno massimo, calcola l'intervallo).

C - Considera il file B (della domanda precedente). (Spiega/motiva entrambe le risposte)<br>
- Il formato di record a lunghezza variabile influisce sull'allocazione?<br>
- Tutti i blocchi allocati memorizzano la stessa quantità di righe di file, data dalla dimensione di un blocco divisa per la lunghezza massima della riga?

---

**Risposte**

A)
 Dati forniti:
* Dim Puntatore/Indici : 32 bit -> 4 byte
* Dim blocchi : 4 KB
* Partizione fs : 1 TB
* file binario : 23033 KB

Il numero di blocchi sarà dato dalla Dim File/ Dim Blocchi = $23033 \times 10^3 / 4 \times 10^3 = 5758.25 \rightarrow 5759$<br>
Calcolo della capacità del blocco indice: Puntatori per blocco indice = Dimensione blocco / Dimensione puntatore = $4 KB / 4 B = 1024 \rightarrow 1K$ puntatori
ma puntatori dati per blocco = Puntatori per blocco indice -1 in quanto in un blocco indice c'è lo slot per il puntantore al blocco indice successivo $\rightarrow 1023$ puntatori
Blocchi indice = Blocchi dati / puntatori dati per blocco = $5729 / 1023 = 5.6 \rightarrow 6$
la frammentazione interna sarà data da (blocchi dati * dimensione blocco) - dim file $\rightarrow$ frammentazione = Dim_blocco * (1- (Dim File % Dim Blocco)/ Dim Blocco) =
$\rightarrow FI_{dati} = 4096 \times (1 - ((23033 \times 1024)\%4096)/4096) = 4096 - 1024 = 3072 \rightarrow 3 KB$
Per i blocchi indice invece avremo che la capacità totale sarà data da numero blocchi * puntatori utili = $6$ blocchi $\times 1023$ puntatori utili = $6138$ slot per puntatori ai dati
quelli effettivamente utilizzati saranno uno per ogni blocco dati = $5759$ puntatori, quelli sprecati li ricaviamo dalla differenza
Puntatori sprecati = $6138 - 5759 = 379$ slot inutilizzati $\Rightarrow$ considerando che ogni slot occupa 4 B $\Rightarrow$
$\Rightarrow$ la frammentazione interna degli indici sarà = $379 \times 4 B = 1516 B$

B)
Ripetiamo i dati:
* Dim Totale = 15300 B; Lunghezza media riga 50 + 1 (51 Bytes); Lunghezza Max 100 + 1 (101 Bytes); minima (per deduzione) >= 1B (solo '\n');
          Numero righe medio = Dimensione file / lunghezza media riga = $15300 / 51 = 300$ B (nessun altro calcolo necessario visto che ci chiede la media)

C) Considera il file B (della domanda precedente):
* Il formato di record a lunghezza variabile influisce sull'allocazione?
**R** - No, a livello di fs vede solo una sequenza di byte da allocare, e la strategia dipenderà in base al fs.
* Tutti i blocchi allocati memorizzano la stessa quantità di righe di file, data dalla dimensione di un blocco divisa per la lunghezza massima della riga?
**R** - No. L'allocazione dei file è gestita a livello del modulo dell'allocazione dei file, mentre le linee del testo del file sono un problema a livello applicativo.
