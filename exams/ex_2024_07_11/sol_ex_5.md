## Domanda 5: Funzioni `sys_exit` e `sys_waitpid` in oS161

Considerare le funzioni `sys_exit` e `sys_waitpid` implementate nel lab4. Rispondere alle seguenti domande (SÌ/NO):

1. Sono già presenti nella versione base di oS161?
2. Sono chiamate di sistema che possono essere chiamate dai processi utente?
3. Nel lab4 un processo deve essere identificato da un `pid`. 
   a. Un `pid` è un puntatore?
   b. Il `pid` è associato a un processo al momento del bootstrap?
   c. La funzione `common_prog` crasha senza implementare e usare `waitpid` (o funzioni correlate)?
4. La funzione `proc_destroy` viene chiamata all'interno di:
   a. la chiamata di sistema `exit`?
   b. la chiamata di sistema `waitpid`?
   c. è chiamata dalla funzione `thread_exit`?

**Risposte**

1. **NO** - fanno parte di quelle funzionalità da implementare per impararne il funzionamento;
2. **NO** - Le funzioni `sys__exit` e `sys_waitpid` sono le implementazioni lato kernel delle system call; le vere system call invocabili dagli user process 
sono `exit` e `waitpid` (senza il prefisso sys_). Gli user process non possono chiamare direttamente le funzioni kernel `sys__exit` e `sys_waitpid`.
3. 
  a. **NO** - Il pid (process identifier) è un intero (tipicamente di tipo `pid_t`), non un puntatore. Serve per identificare univocamente un processo nel sistema.
  b. **NO** - Il pid viene associato al processo quando viene creato (ad esempio, in `proc_create`), non durante il bootstrap del sistema.
  c. **NO** - `common_prog` può funzionare anche senza `waitpid`, ma non sarebbe in grado di attendere correttamente la terminazione dei processi figli e 
  recuperare il loro stato di uscita. Non è obbligatorio che crashi, ma il comportamento sarebbe incompleto.
4. 
  a. **NO** - La system call `exit` (implementata da `sys__exit`) non chiama direttamente `proc_destroy`, perché la struttura del processo deve rimanere per permettere al padre di raccogliere lo stato con `waitpid`.
  b. **YES** - La system call `waitpid` (tramite funzioni come `proc_wait`) chiama `proc_destroy` per liberare la struttura del processo figlio dopo che il padre ha raccolto il suo stato.
  c. **NO** - `thread_exit` si occupa di terminare un thread, non di distruggere la struttura del processo. Non chiama `proc_destroy`.