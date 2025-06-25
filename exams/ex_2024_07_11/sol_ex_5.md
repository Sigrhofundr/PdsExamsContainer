## Domanda 5: Funzioni `sys_exit` e `sys_waitpid` in oS161

Considerare le funzioni `sys_exit` e `sys_waitpid` implementate nel lab4. Rispondere alle seguenti domande:

* Sono già presenti nella versione base di oS161? (SÌ/NO)
* Sono chiamate di sistema che possono essere chiamate dai processi utente? (SÌ/NO)
* Nel lab4 un processo deve essere identificato da un `pid`.
    * Un `pid` è un puntatore? (SÌ/NO)
    * Il `pid` è associato a un processo al momento del bootstrap? (SÌ/NO)
    * La funzione `common_prog` crasha senza implementare e usare `waitpid` (o funzioni correlate)? (SÌ/NO)
* La funzione `proc_destroy` viene chiamata all'interno di:
    * la chiamata di sistema `exit`? (SÌ/NO)
    * la chiamata di sistema `waitpid`? (SÌ/NO)
    * è chiamata dalla funzione `thread_exit`? (SÌ/NO)