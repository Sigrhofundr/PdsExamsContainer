### Domanda 2 - RAID

A) Spiega la struttura RAID, discuti i suoi benefici, spiegando come il livello RAID scelto migliora l'archiviazione e
il recupero dei dati rispetto a un singolo disco. <!--Perché compare nella soluzione ma non nel testo --> Spiega nel dettaglio, RAID 1, mirroring.

B) Nel contesto della struttura RAID, definisci il tempo medio al guasto (MTTF - Mean Time to Failure), il tempo medio di perdita
dei dati (MTTDL - Mean Time to Data Loss) e il tempo medio di riparazione (MTTR - Mean Time to Repair). Fornisci una spiegazione di ciascun termine ed elabora come
queste metriche contribuiscono all'affidabilità complessiva e alla disponibilità di un sistema RAID.

C) - Considera una struttura RAID con 4 dischi in una configurazione mirror (RAID 1).
Supponendo che ogni disco possa guastarsi indipendentemente dagli altri, con un MTTF di 50000 ore e un MTTR di 10 ore,
calcola il MTTDL. Fornisci una spiegazione passo-passo del processo di calcolo.<br>

---

**Risposte**

A) La presenza di più dischi, se il sistema ne permette l'uso in parallelo, rende possibile la frequenza a cui i dati
si possono leggere e scrivere. Una configurazione di questo tipo permette di migliore l'affidabilità della memoria secondaria,
poiché diventa possibile memorizzare le informazioni in più dischi in modo ridondante. In questo modo, un guasto a uno dei
dischi non comporta la perdita immediata dei dati. Esistono delle tecniche per l'organizzazione dei dischi, note col nome di "batterie
ridondanti di dischi" (redundant array of independent disks, RAID), che hanno lo scopo di affrontare i problemi di prestazione
e affidabilità. Come accennato in precedenza, la ridondanza permette di migliorare l'affidabilità dei dischi. Una delle tecniche
consiste nel mirroring, ovvero la duplicazioni del disco in una o più copie identiche al disco originale. (si potrebbe andare più a fondo
al riguardo ma non è richiesto). Il miglioramento delle prestazioni come detto può essere dato dal parallelismo tramite il sezionamento o
striping a livello dei bit, distribuendo i dati in sezioni su più dispositivi. I numerosi schemi per fornire la ridondanza a basso costo, sono stati
divisi in base a costi e prestazioni e sono stati classificati in livelli chiamati livelli RAID. I livelli RAID consistono in dei livelli numerati da
0 a 6 e poi in diverse combinazioni annidate. Alcuni esempi:
* RAID 0 - Divide i dati in blocchi (striping) e li distribuisce su più dischi, aumentando le prestazioni di lettura e scrittura.
  Non offre ridondanza, quindi il guasto di un solo disco comporta la perdita di tutti i dati.
* RAID 1 - Duplica esattamente i dati su due o più dischi (mirroring), garantendo un'elevata tolleranza ai guasti.
  La capacità utilizzabile è pari a quella di un singolo disco, poiché gli altri sono copie.
* RAID 5 - Utilizza lo striping dei dati con parità distribuita su tutti i dischi. Offre un buon equilibrio tra
  prestazioni, ridondanza (può tollerare il guasto di un disco) e utilizzo dello spazio.
* RAID 6 - Simile al RAID 5, ma con doppia parità distribuita, che consente di tollerare il guasto
  contemporaneo di due dischi. Questo aumenta la sicurezza dei dati a discapito di prestazioni di scrittura leggermente inferiori.
* RAID 10 (1+0) - Combina il mirroring (RAID 1) con lo striping (RAID 0). I dati vengono prima specchiati in coppie di
  dischi e poi questi set specchiati vengono strippati, offrendo prestazioni elevate e un'ottima tolleranza ai guasti.

Miglioramenti rispetto a Disco Singolo
Rispetto a un singolo disco, RAID offre throughput moltiplicato per il numero di dischi (in lettura), riduzione drastica
del rischio di perdita dati, continuità operativa durante guasti hardware, e flessibilità nella scelta del compromesso tra prestazioni, sicurezza e costo.
Recap: RAID trasforma più dischi fisici in un sistema logico che supera i limiti di prestazioni e affidabilità dei
singoli dischi, offrendo soluzioni scalabili per diverse esigenze aziendali attraverso livelli configurabili che
bilanciano velocità, sicurezza e capacità utilizzabile.

Il RAID 1 è un livello RAID che garantisce la ridondanza completa dei dati. Funziona creando una copia esatta (mirror) di
tutti i dati su due o più dischi. Questo significa che ogni informazione scritta su un disco viene contemporaneamente
replicata sull'altro (o sugli altri) dischi dell'array. Il vantaggio principale è un'elevatissima tolleranza ai guasti:
se un disco si rompe, i dati sono immediatamente disponibili sull'altro disco, senza interruzioni. Tuttavia, la capacità
utilizzabile del sistema è pari a quella del singolo disco più piccolo, poiché gli altri dischi vengono usati per la duplicazione.

B) Metriche di Affidabilità RAID <br>
Definizioni:<br>
* MTTF (Mean Time To Failure): Tempo medio che intercorre prima che un disco si guasti in modo irrecuperabile.
  Per sistemi RAID, rappresenta l'affidabilità dei singoli componenti e si misura tipicamente in anni.
* MTTDL (Mean Time To Data Loss): Tempo medio necessario perché il sistema subisca una perdita permanente di
  dati dovuta a guasti dei dischi. Dipende dal livello RAID e dalla sua capacità di tollerare guasti multipli.
* MTTR (Mean Time To Repair): Tempo medio richiesto per sostituire un disco guasto e ripristinare completamente l'array,
  includendo il rebuild dei dati.<br>
  Relazioni e Calcoli:<br>
* RAID 0: MTTDL = MTTF_singolo_disco / numero_dischi (diminuisce con più dischi)
* RAID 1: MTTDL = MTTF^2 / (n × MTTR) (molto elevato grazie al mirroring, con N>=2)
* RAID 5: MTTDL = (MTTF²) / (N × MTTR) dove N è il numero di dischi (tolleranza a un guasto)<br>
  Impatto sull'Affidabilità<br>
  Il rapporto MTTF/MTTR determina la finestra di vulnerabilità durante le riparazioni. Un MTTR basso è
  cruciale per sistemi mission-critical, poiché riduce il tempo in cui il sistema opera in modalità degradata.<br>
  La combinazione di queste metriche definisce la disponibilità complessiva del sistema e guida la scelta del
  livello RAID appropriato in base ai requisiti di uptime e protezione dei dati.

C) Utilizzando la formula precedente:<br>
RAID 1: MTTDL = (MTTF²) / (4 × MTTR)  = avendo delle grandezze coerenti posso limitarmi a sostituire ->
-> MTDDL = (50000^2) / 40 = 62,5M ore<br>
Nel caso in cui abbiamo 2 o 4 dischi in una configurazione RAID 1 mirrored, il MTTDL è associato con lo scenario dove entrambi
i dischi in ognuna delle coppie si guasta.
MTTF in un sistema a 4 tipi indipendenti è MTTF/4 e non MTTF/2<br>
Probabilità di Guasto per Sistemi Paralleli<br>
Quando hai n componenti indipendenti che operano in parallelo, il tempo medio al primo guasto del sistema è:
MTTF_sistema = MTTF_singolo_componente / n