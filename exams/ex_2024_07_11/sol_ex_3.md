## Domanda 3: Gestione della Memoria di Massa e I/O

Considerare un sistema informatico che utilizza una combinazione di unità a disco rigido (HDD) e unità a stato solido (SSD) per l'archiviazione di massa. Il sistema incorpora anche vari dispositivi di I/O gestiti da un sottosistema di I/O.

1.  **D** - Una configurazione RAID 0 (striping) può migliorare l'affidabilità dei dati rispetto a una singola configurazione HDD? (SÌ/NO, Spiegare/motivare brevemente) <br>
    **R** - No. Lo striping si riferisce alla parallelizzazione del lavoro tra i dischi attraverso la distribuzione dei dati tra i dischi. Quindi, in caso di guasto di uno dei dischi presenti,
    è probabile che i dati di quel disco vengano persi, quindi NON è affidabile. Il RAID 0 migliora le performance ma non la stabilità.<br>
2.  **D** - È possibile che un SSD soffra di problemi di frammentazione simili a quelli riscontrati dagli HDD? (SÌ/NO, Spiegare/motivare brevemente)<br>
    **R** - No. Gli SSD non soffrono di problemi di frammentazione simili agli HDD perché l'accesso ai dati è elettronico e non meccanico. Il tempo di accesso è uniforme indipendentemente dalla posizione fisica dei blocchi.
    Anche se i file possono essere frammentati logicamente, questo non causa degradi prestazionali significativi come negli HDD dove la frammentazione richiede costosi movimenti fisici della testina. Gli SSD hanno problemi
    diversi (write amplification, garbage collection) ma non la frammentazione classica che affligge i dischi meccanici.
3.  **D** - È sempre più efficiente utilizzare il Direct Memory Access (DMA) per tutti i tipi di operazioni di I/O rispetto all'I/O guidato da interrupt? (SÌ/NO, Spiegare/motivare brevemente)<br>
    **R** - No. La DMA è generalmente più efficiente per i trasferimenti di grandi quantità di dati, ma per quelli piccoli, operazioni veloci, interrupt-driven I/O può essere più efficiente a causa dell'overhead associato alla configurazione iniziale del DMA.
4.  **D** - Il polling è un metodo più efficiente per gestire le operazioni di I/O rispetto all'utilizzo degli interrupt in un ambiente di rete ad alta velocità? (SÌ/NO, Spiegare/motivare brevemente)<br>
    **R** - No. Gli interrupt sono più efficienti in ambienti di rete ad alta velocità perché riducono l'utilizzo della CPU rispetto al polling, che controlla continuamente lo stato del dispositivo I/O.
5.  **D** - Uno scheduler di I/O ben progettato può migliorare significativamente le prestazioni di una configurazione RAID 5 rispetto a uno scheduler di I/O mal progettato? (SÌ/NO, Spiegare/motivare brevemente)<br>
    **R** - Sì. Uno scheduler I/O ben progettato può ottimizzare l'ordine delle operazioni read/write, riducendo i seek time e migliorare le prestazioni complessive, cosa particolarmente vantaggiosa in RAID 5
    dove calcoli di parità e dati distribuiti richiedono una gestione efficiente.
6.  **D** - È possibile che un'operazione di I/O su un SSD abbia sempre una latenza inferiore rispetto alla stessa operazione su un HDD? (SÌ/NO, Spiegare/motivare brevemente)<br>
    **R** - Sì, gli SSD generalmente hanno latenza inferiore grazie all'assenza di parti mobili. Tuttavia, esistono rari casi specifici dove un HDD potrebbe avere prestazioni migliori.