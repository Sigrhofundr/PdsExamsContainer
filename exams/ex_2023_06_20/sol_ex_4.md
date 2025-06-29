## Domanda 4

**IMPORTANTE:** TUTTE LE RISPOSTE SÌ / NO DEVONO ESSERE MOTIVATE. QUANDO I RISULTATI SONO NUMERI, SONO NECESSARI IL RISULTATO FINALE E I RELATIVI PASSI INTERMEDI (O FORMULE).

Dato un sistema OS161.

A) Si considerino le funzioni `thread_switch` e `thread_fork`. Quale viene utilizzata per creare un processo? Quale per creare un nuovo kernel thread? (motivare).

B) Quando viene creato un nuovo kernel thread in OS161, questo appartiene al processo che lo ha creato o può appartenere a un diverso processo? (motivare).

C) Perché la funzione `thread_exit()`, nella versione base di OS161, non può essere chiamata dopo che si è chiamata `proc_destroy` (per il processo a cui appartiene il thread)?.

D) Cosa succede se non viene chiamata la funzione `as_destroy` al termine dell’esecuzione di un processo user? Il kernel può andare in crash? (motivare).

---

**Risposte**

A) Nello specifico nessuna delle due viene utilizzata per creare un processo vero e proprio su OS161. La creazione del processo avviene tramite la sys call `fork()`.
La `thread_fork` creerà un nuovo thread, andando a richiamare le diverse funzioni per la corretta creazione. 
Dopo che questo thread verrà inserito alla coda dei thread ready, attraverso la funzione `threadlist_addtail()`, quando lo scheduler lo deciderà verrà lanciato, richiamando la
`thread_switch` per effettuare il cambio di contesto tra il thread precedentemente running a quello nuovo.

B) La funzione `thread_fork()` accetta come parametro un puntatore alla struttura `struct proc` (`struct proc* proc`). 
Questo parametro viene utilizzato per specificare il processo a cui il nuovo thread appartiene. Pertanto, un thread può essere assegnato 
a un processo specifico al momento della sua creazione, incluso il processo che ha chiamato `thread_fork()` o un altro processo esistente 
(ad esempio, per creare thread per demoni o altri compiti del kernel che non sono associati a un processo utente specifico).
Tuttavia, una volta creato, un thread è strettamente legato al suo processo di appartenenza e ne condivide lo spazio di indirizzi e le risorse.

C) La funzione `proc_destroy()` ha lo scopo di liberare le risorse associate a un processo dopo che tutti i suoi thread hanno terminato la loro esecuzione. 
La sua implementazione include un controllo di integrità (`KASSERT`) che verifica se il conteggio dei thread attivi all'interno del processo è zero.
Se `proc_destroy()` venisse chiamata mentre ci sono ancora thread attivi (come avverrebbe se chiamassimo `thread_exit()` dopo), il `KASSERT` (o un controllo simile) andrebbe in fallimento, causando un `panic()`. 
Questo è un meccanismo di sicurezza per prevenire l'accesso a strutture dati già liberate o il rilascio di risorse ancora in uso.
In breve, il processo di distruzione di un processo (`proc_destroy`) deve essere l'ultimo passo, eseguito solo dopo che tutti i thread a esso associati hanno terminato la loro vita utile e hanno liberato le loro risorse a livello di thread (tramite `thread_exit` o un meccanismo equivalente). 
<!-- risposta sintetica data inizialmente** Perché la `proc_destroy` genera `panic()` nel caso in uci il processo abbia dei thread ancora attivi.-->

D) Se la funzione `as_destroy` non viene chiamata al termine dell'esecuzione di un processo utente, il kernel non andrà in crash immediatamente, ma si verificherà un grave memory leak.
La funzione `as_destroy` ha il compito di liberare tutte le pagine di memoria fisica e le strutture dati del kernel associate allo spazio degli indirizzi del processo utente (address space). 
Se questa funzione non viene invocata:
- Le pagine di memoria fisica precedentemente assegnate al processo non vengono restituite al pool di memoria libera del kernel. 
- Le strutture dati interne del kernel che mappano la memoria virtuale a quella fisica (as, pagetable, ecc.) rimangono allocate in memoria.

Di conseguenza, il kernel non potrà riutilizzare queste risorse. Con l'esecuzione di più processi, la memoria disponibile si esaurirà 
progressivamente fino a un punto in cui il kernel non sarà più in grado di allocare nuove pagine o strutture dati, causando un malfunzionamento grave e,
in definitiva, un crash del sistema (panic) per esaurimento delle risorse.

Quindi, il crash non è immediato ma è una conseguenza inevitabile della saturazione della memoria dovuta alla mancata liberazione delle risorse.
<!-- risposta sintetica data inizialmente** La funzione `as_destroy` si occupa, per quanto riguarda il rilascio lato utente, di liberare lo spazio degli indirizzi ricevuto in input. Quindi potrebbe non crashare immediatamente, 
ma arriverebbe a saturare la memoria velocemente. -->