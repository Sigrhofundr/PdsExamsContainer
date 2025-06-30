### Domanda 4

Si fornisce una rappresentazione (parziale) della funzione `Os161 thread_fork`.

```c
thread_fork (const char *name, struct proc *proc,
             void (*entrypoint) (void *data1, unsigned long data2),
             void *data1, unsigned long data2) {

    struct thread *newthread;
    int result;

    newthread = thread_create (name);

    /* Allocate a stack */
    newthread->t_stack = kmalloc(STACK_SIZE);

    /* Thread subsystem fields */
    newthread->t_cpu = curthread->t_cpu;

    /* Attach the new thread to its process */
    result = proc_addthread (proc, newthread);

    /* Set up the switchframe so entrypoint() gets called */
    switchframe_init(newthread, entrypoint, data1, data2);

    /* Lock the current cpu's run queue and make the new thread runnable */
    thread_make_runnable (newthread, false);

    return 0;
}
```
Basandosi sul codice riportato e su quanto appreso in lezioni e laboratori, si risponda alle seguenti domande:

A) Che cos'è entrypoint e a cosa serve nel contesto della creazione di un thread?
B) Lo stack allocato (di dimensione STACK_SIZE) è stack kernel oppure user? (motivare)
C) Che cosa fa la funzione thread_make_runnable? Manda in esecuzione il thread? (se no, motivare, se sì, quando?)
D) È possibile che l'esecuzione arrivi all'istruzione finale (return 0;), oppure, una volta entrati
in thread_make_runnable, non è possibile che il controllo sia ritornato alla thread_fork?

---

**Risposte**
A) L'`entrypoint` è un puntatore alla funzione che il nuovo thread dovrà eseguire quando verrà avviato. Viene passato a `switchframe_init`, 
che predispone il contesto di esecuzione del nuovo thread in modo che, quando sarà schedulato, inizi l’esecuzione proprio dalla funzione indicata da `entrypoint`.
B) Lo stack allocato è uno stack kernel. Questo perché il thread viene creato e gestito dal kernel, e la funzione `kmalloc` alloca memoria nello 
spazio del kernel. Lo stack utente viene invece creato solo quando il thread passa in modalità utente, ad esempio tramite la funzione `enter_new_process`.;
C) la funzione `thread_make_runnable` inserisce il nuovo thread nella coda dei thread "ready" (pronti all’esecuzione). 
Non lo esegue immediatamente: sarà il sistema di scheduling del kernel a decidere quando il thread verrà effettivamente eseguito, 
in base alle politiche di gestione dei thread e alla situazione della CPU.;
D) Sì, l’esecuzione arriva all’istruzione finale return 0; e la funzione ritorna normalmente al chiamante. 
Questo perché `thread_make_runnable` si limita a mettere il nuovo thread nella coda dei "ready", ma il thread chiamante (thread_fork) prosegue la sua 
esecuzione e termina normalmente. Non bisogna confondere questa situazione con funzioni come enter_new_process, 
che invece non ritornano mai al chiamante, ma trasferiscono il controllo direttamente al nuovo processo utente.