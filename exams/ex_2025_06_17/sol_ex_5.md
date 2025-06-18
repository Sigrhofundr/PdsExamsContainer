## Domanda 5 (comprende le domande da 14 a 16 del questionario)

Si consideri, in OS161, l'implementazione della chiamata di sistema waitpid realizzata nel laboratorio 4.

Rispondere SÌ/NO alle seguenti domande.

### Domanda 14

**Punteggio max.: 1,00**

La funzione `thread_exit()`

* distrugge la struttura dati del thread e restituisce il puntatore al processo padre
* decrementa il contatore dei thread in un processo
* cancella (assegnando NULL) il puntatore al thread nella struttura del processo
* chiama la funzione `proc_rem_thread ()`

---

### Domanda 15

**Punteggio max.: 1,00**

La funzione `proc_destroy()`

* può essere chiamata all'interno della chiamata di sistema SYS_EXIT, dopo aver chiamato `thread_exit()`, se il numero di thread nel processo diventa 0
* non può essere chiamata prima di `thread_exit()`, a meno che tutti i thread siano stati staccati dal processo e lo stato di uscita sia stato letto
* attende la terminazione del processo, quindi libera la struttura del processo
* legge lo stato di uscita e lo restituisce al chiamante

---

### Domanda 16

**Punteggio max.: 1,00**

La system call `waitpid()`

* riceve come parametro l'identificatore (PID) del processo da attendere
* attende un cambio di stato di un processo (in lab4 viene gestito solo lo stato di uscita/exit)
* può essere chiamato in OS161 dalla funzione `common_prog ()`, chiamata dal menu dopo aver creato un processo utente
* può chiamare `proc_destroy (curproc)`, dopo che il semaforo utilizzato per attendere (o un’altra
* primitiva) è stato segnalato.
