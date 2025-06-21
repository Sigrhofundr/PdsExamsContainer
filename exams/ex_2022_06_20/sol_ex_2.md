## Domanda 2

Un processo user vuole effettuare un input da un dispositivo di IO a caratteri (character device)
mediante una strategia basata su polling del dispositivo. Per evitare un'attesa troppo lunga (nel
loop di polling) da parte del driver del dispositivo, si chiede all'autore del programma eseguito dal
processo user, di fare direttamente polling mediante un loop di lettura del registro di stato del
dispositivo.

A) È possibile effettuare questa operazione? (se si, dire come, se no, dire perche)

B) Supponendo che il dispositivo sia associato alla tastiera e lo si gestisca in interrupt, ci si
aspetta che venga generato un interrupt per ogni carattere ricevuto oppure un interrupt per
ogni "invio" (cioè enter, fine-riga)? (motivare)

Dato un altro dispositivo di I/O, gestito a blocchi:

C) È possibile che una sola scrittura (mediante chiamata a _write()_), tenti di fare output di piu di
una coppia puntatore/dimensione?

D) Dato il driver che realizza la scrittura, supponendo che il sistema sia dotato di DMA
controller, e possibile che venga eseguita "prima" una operazione di lettura e/o scrittura
attivata successivamente a questa, da un altro processo? E (stessa domanda) dallo
stesso processo? (motivare)

**Risposte**

A) I processori moderni implementano livelli di privilegio per proteggere il sistema. I processi user eseguono in user mode con accesso limitato, mentre l'accesso ai registri hardware è riservato al kernel mode. 
Questa architettura impedisce ai processi user di accedere direttamente ai registri dei dispositivi I/O, richiedendo l'uso di system call per le operazioni I/O. 
La protezione è fondamentale per garantire sicurezza, stabilità e isolamento tra processi.

B) La tastiera genera un interrupt per ogni carattere essendo un dispositivo a caratteri. Questo garantisce reattività massima, mentre la bufferizzazione per correzioni (backspace, editing di linea) viene gestita via software dal driver/SO. 
L'approccio per-carattere è necessario perché l'hardware della tastiera standard non ha capacità di buffering intelligente integrata.

C) La write() standard riceve una sola coppia puntatore/dimensione per mantenere semplicità d'uso. 
La gestione di vettori di operazioni multiple avviene a livello più basso (VOP_READ/VOP_WRITE) per ottimizzazioni interne, oppure tramite system call specializzate come writev(). Questa separazione garantisce interfacce user-friendly mantenendo efficienza a livello kernel.

D) L'ordine con cui i processi chiedono di leggere o scrivere non determina l'ordine con cui le operazioni vengono fisicamente eseguite. Questa decisione spetta allo Scheduler di I/O, che agisce a un livello basso per ottimizzare le prestazioni, riordinando le richieste presenti in una coda. Questo è reso possibile dall'uso del DMA e può avvenire tra processi diversi o all'interno dello stesso processo se questo usa I/O asincrono o è multithreaded.