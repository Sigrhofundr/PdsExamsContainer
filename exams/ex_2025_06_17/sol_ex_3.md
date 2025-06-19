## Domanda 3 (comprende le domande da 9 a 11 del questionario)

Rispondere SI/NO alle seguenti domande sulle memorie di massa e sul file system.

### Domanda 9

**Punteggio max.: 1,00**

Rispondere alle seguenti domande sulle strutture dati del file system.

* Il requisito principale per l'implementazione delle directory è l'efficienza nel creare e rimuovere (delete) un file **NO**

Il requisito principale è l'efficienza nella ricerca dei file. Le operazioni di lookup sono molto più frequenti di create/delete. Le directory devono ottimizzare la ricerca tramite hash table, B-tree, etc.

* Una per-process file table è allocata nello spazio kernel **SÌ**

La per-process file table fa parte delle strutture dati del kernel per ogni processo. Contiene i file descriptor e puntatori alla system-wide table. È in kernel space per sicurezza e controllo degli accessi.

* L'operazione di apertura di un file (open) può modificare sia la per-process file table che la system-wide **SÌ**

L'open() crea una nuova entry nella per-process table (nuovo fd) e può creare/incrementare riferimenti nella system-wide table (se il file non era già aperto da altri processi).

* Il puntatore alla posizione corrente di lettura/scrittura in un file aperto risiede nel blocco di controllo del file (file control block) **NO**

Il file offset pointer risiede nella per-process file table, non nel FCB. Ogni processo ha il proprio offset indipendente per lo stesso file. Il FCB contiene metadati permanenti del file.

### Domanda 10

**Punteggio max.: 1,00**

Considerare le possibili implementazioni delle chiamate di sistema read/write, rispondere alle seguenti domande.

* L'accesso casuale (diretto) può essere simulato dall'accesso sequenziale **SÌ**

Si può simulare accesso casuale riavvolgendo e leggendo sequenzialmente fino alla posizione desiderata. Inefficiente ma possibile (es. nastri magnetici).

* L'accesso casuale (diretto) non può essere implementato tramite formati di allocazione file basati su liste concatenate (ad esempio FAT, lista di blocchi) **NO**

Le liste concatenate supportano accesso casuale, ma con overhead. La FAT mantiene la catena in memoria per accesso diretto. Le linked list richiedono traversal ma è comunque possibile.

* L'accesso casuale (diretto) può essere implementato da tutti i formati di allocazione dei file **SÌ**

Contiguous, indexed, e linked supportano tutti accesso casuale, con diversi costi. Contiguous è O(1), indexed è O(log n), linked è O(n) ma tutti lo supportano.

* L'accesso sequenziale non può essere simulato dall'accesso casuale (diretto) **NO**

L'accesso sequenziale è facilmente simulabile con accesso casuale incrementando progressivamente l'offset. È una simulazione perfetta ed efficiente.

### Domanda 11

**Punteggio max.: 1,00**

Rispondere alle seguenti domande sull'organizzazione dei dischi RAID.

* Il mirroring include bit di parità **NO**

Il mirroring (RAID 1) usa duplicazione completa dei dati, non bit di parità. I bit di parità sono usati in RAID 3, 4, 5, 6 per recupero tramite calcoli XOR.

* Dati N dischi in configurazione striping, se tutti i dischi hanno un dato MTTF, la probabilità di un guasto diminuisce con l'aumento del valore di N **NO**

Con più dischi, la probabilità di guasto aumenta. MTTF del sistema = MTTF_singolo / N. Più componenti = più punti di failure = maggiore probabilità di guasto.

* Sono date due configurazioni  RAID: A con 2 dischi in mirroring e B con 4 dischi (striping + mirroring).  Tutti i dischi hanno lo stesso MTTF. L'MTTTDL (perdita di dati) di B è superiore all'MTTTDL di A **NO**

RAID 1+0 (4 dischi) ha MTTTDL inferiore a RAID 1 semplice. Più dischi = più possibili combinazioni di failure che portano a perdita dati. B è meno affidabile di A.

* Dopo un guasto, l'utilizzo del sistema deve essere interrotto fino al completamento della riparazione **NO**

I sistemi RAID con ridondanza permettono operazione degradata durante la ricostruzione. Il sistema continua a funzionare (con prestazioni ridotte) mentre si ripara in background.

---
