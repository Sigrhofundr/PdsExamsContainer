## Domanda 5 (comprende le domande da 14 a 16 del questionario)

Si consideri, in OS161, l'implementazione della chiamata di sistema waitpid realizzata nel laboratorio 4.

Rispondere SÌ/NO alle seguenti domande.

### Domanda 14

**Punteggio max.: 1,00**

La funzione `thread_exit()`

* distrugge la struttura dati del thread e restituisce il puntatore al processo padre - **No** - non restituisce alcun valore, né restituisce il puntatore al processo padre. 
La funzione si occupa invece di terminare il thread corrente e di eseguire le operazioni di cleanup necessarie, ma la relazione con il padre 
viene gestita a un altro livello (di solito nella logica di waitpid/exit), non direttamente qui.
* decrementa il contatore dei thread in un processo -**Sì** - tramite la `proc_remthread ()` (senza il tratto basso è la funzione corretta)
* cancella (assegnando NULL) il puntatore al thread nella struttura del processo - **Sì** - Quando un thread viene rimosso dal processo, il puntatore al thread nella struttura dati del processo viene tipicamente impostato a NULL, per evitare riferimenti pendenti a thread non più esistenti.
* chiama la funzione `proc_rem_thread ()` - **Sì** - per quanto detto anche prima, rimuove il thread dalla lista dei threads attivi del processo corrente;

---

### Domanda 15

**Punteggio max.: 1,00**

La funzione `proc_destroy()`

* può essere chiamata all'interno della chiamata di sistema SYS_EXIT, dopo aver chiamato `thread_exit()`, se il numero di thread nel processo diventa 0 - **No** - Chiamare `proc_destroy()` subito dopo
`thread_exit()` (cioè direttamente dentro `SYS_EXIT`) violerebbe il meccanismo di raccolta del codice di uscita, perché il padre potrebbe non aver ancora potuto fare `waitpid()`.
* non può essere chiamata prima di `thread_exit()`, a meno che tutti i thread siano stati staccati dal processo e lo stato di uscita sia stato letto - **Sì** - Analogamente, Non è sicuro chiamare
`proc_destroy()` prima che tutti i thread siano terminati e staccati dalla struttura del processo, altrimenti si rischia di invalidare riferimenti a memoria ancora in uso. 
In particolare, in presenza di waitpid, bisogna anche assicurarsi che lo stato di uscita (exit status) sia stato raccolto dal padre, per evitare la perdita di informazioni.
* attende la terminazione del processo, quindi libera la struttura del processo - **No** - `proc_destroy()` non si occupa di attendere la terminazione del processo; 
viene chiamata **dopo** che il processo è già terminato (cioè quando non ci sono più thread attivi). Il suo scopo è solo quello di liberare la struttura dati e le risorse associate al processo.
* legge lo stato di uscita e lo restituisce al chiamante - **No** - La funzione `proc_destroy()` non si occupa di leggere o restituire lo stato di uscita del processo.
  La gestione e la consegna dell’exit status sono responsabilità di altre funzioni, come `waitpid()`.

---

### Domanda 16

**Punteggio max.: 1,00**

La system call `waitpid()`

* riceve come parametro l'identificatore (PID) del processo da attendere - **Sì** - La firma della system call `waitpid()` prevede che uno dei parametri sia proprio il PID del processo figlio di cui si vuole attendere la terminazione.
* attende un cambio di stato di un processo (in lab4 viene gestito solo lo stato di uscita/exit) - **Sì** - `waitpid()` serve ad attendere che un processo cambi stato (tipicamente la terminazione)
* può essere chiamato in OS161 dalla funzione `common_prog()`, chiamata dal menu dopo aver creato un processo utente - **No** - Solo processi utente possono chiamare la system call waitpid, non funzioni del kernel come common_prog
* può chiamare `proc_destroy(curproc)`, dopo che il semaforo utilizzato per attendere (o un’altra
primitiva) è stato segnalato. - **No** - `waitpid()` deve chiamare `proc_destroy()` sul processo figlio terminato, non su `curproc` che è il processo padre chiamante.
  Distruggere `curproc` sarebbe un errore logico/funzionale.


> [!CAUTION]
> c'è stato molto dibattito su queste risposte secche date dal professore, ho provato a giustificarle ma in alcuni casi non vi è la certezza
