## Domanda 3 (comprende le domande da 9 a 11 del questionario)

Rispondere SI/NO alle seguenti domande sulle memorie di massa e sul file system.

### Domanda 9

**Punteggio max.: 1,00**

Rispondere alle seguenti domande sulle strutture dati del file system.

* Il requisito principale per l'implementazione delle directory è l'efficienza nel creare e rimuovere (delete) un file
* Una per-process file table è allocata nello spazio kernel 
* L'operazione di apertura di un file (open) può modificare sia la per-process file table che la system-wide 
* Il puntatore alla posizione corrente di lettura/scrittura in un file aperto risiede nel blocco di controllo del file (file control block)

### Domanda 10

**Punteggio max.: 1,00**

Considerare le possibili implementazioni  delle chiamate di sistema read/write, rispondere alle seguenti domande.

* L'accesso casuale (diretto) può essere simulato dall'accesso sequenziale
* L'accesso casuale (diretto) non può essere implementato tramite formati di allocazione file basati su liste concatenate (ad esempio FAT, lista di blocchi)
* L'accesso casuale (diretto) può essere implementato da tutti i formati di allocazione dei file 
* L'accesso sequenziale non può essere simulato dall'accesso casuale (diretto)

### Domanda 11

**Punteggio max.: 1,00**

Rispondere alle seguenti domande sull'organizzazione dei dischi RAID.

* Il mirroring include bit di parità
* Dati N dischi in configurazione striping, se tutti i dischi hanno un dato MTTF, la probabilità di un guasto diminuisce con l'aumento del valore di N
* Sono date due configurazioni  RAID: A con 2 dischi in mirroring e B con 4 dischi (striping + mirroring).  Tutti i dischi hanno lo stesso MTTF. L'MTTTDL (perdita di dati) di B è superiore all'MTTTDL di A 
* Dopo un guasto, l'utilizzo del sistema deve essere interrotto fino al completamento della riparazione

---
