## Domanda 3

_(SÌ/NO, Spiega/motiva brevemente)_

1. In un sistema con più dispositivi I/O e una singola CPU, le operazioni di I/O possono avvenire in
   parallelo se la CPU è impegnata nell'esecuzione di un processo?
2. In un sistema con memoria virtuale a paginazione su richiesta e DMA, un page fault può verificarsi
   durante un trasferimento DMA, causando il fallimento del trasferimento se non vengono prese le dovute precauzioni?
3. In un sistema che utilizza I/O basato su interrupt, è possibile che un interrupt venga perso
   se il controller degli interrupt è occupato a elaborare un altro interrupt e il dispositivo che ha attivato il secondo
   interrupt non supporta la coda degli interrupt?
4. Considera un Hard Disk Drive (HDD) con le seguenti specifiche:
    * Dimensione del settore: 512 Byte
    * Numero di tracce per superficie: 5.000
    * Numero di settori per traccia: 300
    * Numero di piatti a doppia faccia: 6
    * Velocità di rotazione del piatto: 7.200 rpm (giri al minuto)
    1. Calcola la massima velocità di trasferimento dati possibile in megabyte al secondo (MB/s),
       assumendo che una traccia di dati possa essere trasferita per rivoluzione.
    2. Se il disco subisce un crash della testina su un piatto, come influisce questo sulla capacità totale e
       sulla disponibilità dei dati, assumendo che non siano presenti meccanismi RAID o di backup?
    3. Se il disco ha un tempo di seek medio di 4 ms e deve leggere un file di 350MB diviso su 200 tracce non
       contigue, calcola il tempo totale richiesto per leggere il file. Includi il tempo di seek, la latenza rotazionale e il
       trasferimento dati. Supponi che la latenza rotazionale media sia di 4,165 ms e che una traccia possa essere letta per rivoluzione.
>[!NOTE]
> Questa ultima domanda è la versione plausibile indicata nella soluzione anche dallo stesso Prof. (non era corretta proprio la domanda)
 
---

### Risposte

1. **Sì**. Le operazioni I/O possono essere gestite da controllori I/O dedicati (ad esempio DMA), permettendo loro
   di procedere indipendentemente dalla CPU, la quale può continuare a eseguire processi mentre le operazioni I/O procedono;
2. **Sì**. Il page fault non è causato dal trasferimento del DMA, dato che il DMA gestisce solo indirizzi fisici, e
   quindi fisicamente contigui in memoria, ma una page fault può essere innescata da un codice (dello stesso o di un altro
   processo) eseguito in parallelo. Se ciò accade, le pagine coinvolte nel DMA non dovrebbero essere selezionate come vittime
   per la sostituzione. Il sistema operativo dovrebbe pertanto o bloccare le pagine in memoria durante il trasferimento del DMA
   oppure gestire i page fault in maniera tale da non impedire il corretto funzionamento del processo DMA;
3. **Sì**. Se il controller degli interrupt è occupato nella gestione di un altro interrupt, e il dispositivo che ha
   innescato il secondo interrupt non supporta l'accodamento dell'interrupt, quest'ultimo andrà perso. Questo accade perché il
   controller dell'interrupt potrebbe non essere capace di registrare i nuovi inputs mentre gestisce quello corrente, e senza
   l'accodamento, il dispositivo non può gestire la richiesta dell'interrupt fino a quando il controllore è pronto;
4. Premettendo che stiamo assumendo che i dati non vengano letti/scritti in parallelo, calcoliamo:
    1. Rivoluzioni per secondo = RPM / 60 = 7200/60 = 120 giri al secondo;<br> Dati per traccia = num settori per traccia * dimensione del settore = 300 * 512 = 153600 B ≈ 0.1465 MB<br>
       Il throughput massimo = Dati per traccia * rivoluzioni per secondo = 0.1465 MB * 120 ≈ 17.57 MB/s
    2. Tenendo conto che 6 platter double-sided = 12 superfici totali;  Capacità per superficie = Tracks × Sectors per track × Sector size
       = 5000 * 300 * 512 B = 768MB => Capacità complessiva 768MB * 12 = 9,216GB<br>
       Se il crash coinvolge un platter completo (entrambi i lati):
       Superfici perse: 2 (entrambi i lati del platter)
       Capacità persa: 2 × Capacità per superficie
       Percentuale di perdita: 2/12 = 16.67% della capacità totale => in spazio (meno la %) circa 7,67 GB rimanenti. I dati sul piatto, se non esiste una copia del backup, saranno irrimediabilmente persi
    3. Calcoliamo:
        * Dimensione del file = 350 MB = 350 * 1024 * 1024 = 367.001.600 Bytes
        * Data per track: dimensione_file / numero_tracce = 350 MB / 200 = 1.75 MB per traccia
        * Transfer_rate_per_track = Capacity_per_track / Time_per_revolution = 153600/ 8,33 ms = 18432 MB/s
        * Total_Transfer_time_ = Data_per_track / (Transfer_rate_per_track* Number_of_tracks) = 1.75 MB / (18432 MB/s*200) = 18,99 sec
        * Total_Seek_Time = Number_of_tracks × Average_seek_time = 200 * 4 ms = 0,8 sec
        * Total_Rotational_Latency = Number_of_tracks × Average_rotational_latency = 200 * 4165 ms = 0,833 sec
        * Transfer_time_per_track = 60 / RPM (tempo per una rivoluzione completa) = 8,3 ms
        * Total_Time = Total_Seek_Time + Total_Rotational_Latency + Total_Transfer_Time = 18,99 + 0,833 + 0,8 = 20,62 sec

>[!NOTE]
> Dato quanto indicato anche nella domanda, la risposta riportata dal prof sarà diversa (si raccomanda di controllare il file originale)
