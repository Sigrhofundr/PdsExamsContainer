### Domanda 3

Si consideri il problema della gestione di una richiesta di IO a blocchi realizzato mediante DMA.<br>
A) Che cosa si intende, in questo contesto, con il termine “cycle stealing”?<br>
B) Perché il trasferimento in DMA è vantaggioso rispetto all’IO programmato?<br>
C) Immaginando di dover trasferire 10MB di dati da disco a memoria RAM, quanti Byte transitano sul bus dati della RAM nei
due casi (trasferimento in DMA e IO programmato)? (motivare) <br>
D) Qualora si volesse usare per l’IO, mediante DMA, un buffer in memoria Kernel, invece di usare direttamente la
sorgente/destinazione in memoria user, il DMA controller farebbe più o meno trasferimenti, oppure lo stesso numero, rispetto alla versione senza buffer? (motivare)

---

**Risposte**

A) Nel contesto del DMA, il “cycle stealing” è la tecnica con cui il controller DMA acquisisce il controllo del bus della memoria 
per trasferire dati tra un dispositivo di I/O e la RAM, sottraendo temporaneamente l’accesso alla memoria alla CPU. 
In questi momenti, la CPU deve aspettare che il DMA completi l’operazione sul bus prima di poter accedere alla RAM, 
rallentando il proprio ciclo di esecuzione. Tuttavia, il vantaggio complessivo è che la CPU è liberata dalla gestione diretta dei trasferimenti di dati, migliorando l’efficienza del sistema.

B) L’uso del DMA è vantaggioso rispetto all’I/O programmato principalmente perché:
* Efficienza della CPU: la CPU non è più bloccata nella gestione del trasferimento byte per byte, ma può dedicarsi ad altre operazioni mentre il DMA gestisce autonomamente il trasferimento di blocchi di dati.
* Riduzione del traffico su bus e registri della CPU: i dati passano direttamente tra RAM e dispositivo senza dover transitare 
attraverso i registri della CPU, riducendo il numero di trasferimenti complessivi e il carico sulla CPU e sul bus.
* Migliore throughput: l’operazione di I/O risulta più veloce e il sistema può raggiungere una maggiore multiprogrammazione.

<!-- prima versione
A) Per il trasferimento dei file, il DMA avvia una fase di handshaking tra il controller del DMA e quello del dispositivo per avviare una connessione su
due fili (DMA-request e DMA-acknowledge). Il controller del dispositivo manda un segnale sulla linea DMA-request quando una word di dati è disponibile per il trasferimento.
Questo segnale fa sì che il controllore DMA prenda possesso del bus di memoria, presenti l'indirizzo desiderato e mandi un segnale lungo la linea DMA-acknowledge.
Quando il controllore del dispositivo ricevere questo segnale, trasferisce in memoria la word di dati e rimuove il segnale dalla linea DMA-request.
Quando l'intero trasferimento termina, il controllore del DMA invia un interrupt alla CPU. A questo punto il controller della DMA "ruba" il possesso del bus di memoria, 
e la CPU è temporaneamente impossibilitata ad accadere alla memoria centrale (può comunque accedere alla cache primaria e secondaria); questo potrebbe causare dei rallentamenti 
nelle computazioni della CPU; ciononostante l'assegnamento del lavoro di trasferimento di dati a un controllore DMA migliora in generale le prestazioni complessive del sistema.

B) Si tende a preferire l'utilizzo di una DMA in quanto sarebbe uno spreco di risorse della CPU eseguire singolarmente tutte le operazioni di I/O (controllo dei bit di stato, scrittura 
di dati nel registro del controller un byte alla volta - programmed I/O - PIO, trasferimento di file di grandi dimensioni), pertanto la CPU si limita a fornire alla DMA i comandi 
necessari alle operazioni (il comando include gli indirizzi di sorgente e destinazione, la modalità, lettura o scrittura, il numero di bytes). In questo modo delega queste operazioni e 
può dedicarsi ad altro, ricevendo un interrupt al termine delle operazioni.
-->
C)
* **DMA**:
In modalità DMA, i dati vengono trasferiti direttamente dal dispositivo di I/O alla RAM, attraversando il bus dati della memoria una sola volta per ogni byte trasferito.
    Quindi, per 10MB, transitano esattamente **10MB** di dati sul bus della RAM.

* **I/O Programmato**:
Nell’I/O programmato, la CPU legge i dati dal dispositivo (tramite i suoi registri) e poi li scrive in RAM. Per ogni byte trasferito:
  - La CPU legge il byte dal registro di I/O (operazione interna alla CPU).
  - La CPU scrive il byte nella RAM (tramite il bus dati della memoria). Tuttavia, spesso il trasferimento coinvolge due passaggi separati sul bus della memoria:
    - Primo passaggio: la CPU legge (dal bus dati) il valore del registro di I/O in un registro interno.
    - Secondo passaggio: la CPU scrive (sul bus dati) il valore dal registro interno alla RAM. In termini pratici, ogni byte viene trasferito due volte sul bus dati della RAM: una per la lettura (dal registro I/O al registro CPU) e una per la scrittura (dal registro CPU alla RAM).

Totale: Per 10MB di dati, transitano **20MB** di dati sul bus della RAM.

D) Il DMA controller effettua lo stesso numero di trasferimenti rispetto al caso senza buffer kernel, ma la differenza sta nella destinazione/sorgente in RAM:
* Con buffer in kernel: il DMA scrive (o legge) i dati dal buffer in memoria del kernel (area protetta).
* Senza buffer kernel: il DMA scrive (o legge) direttamente dalla memoria dell’applicazione user.

In entrambi i casi, il DMA effettua il trasferimento del blocco di dati tra il dispositivo e la RAM, e il numero di trasferimenti via DMA è lo stesso (ad esempio, 10MB = 10MB trasferiti via DMA).

Tuttavia, nel caso del buffer kernel, ci sarà un passaggio aggiuntivo (non gestito da DMA, ma dalla CPU) per copiare i dati dal 
buffer kernel alla memoria user (o viceversa). Questo secondo trasferimento avviene in modo separato dal DMA, per ragioni 
di sicurezza e protezione della memoria, ma non implica un aumento dei trasferimenti DMA, solo un’ulteriore copia gestita dal sistema operativo.