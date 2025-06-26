### Domanda 5

A) Si consideri la funzione `sys_getpid` (prototipo qui sotto) implementata nel lab4:

`pid_t sys_getpid(void);`

Per ciascuna delle seguenti affermazioni, rispondere vero/falso (senza spiegazione).

1.  La funzione è necessaria per implementare `sys_waitpid`, perché è necessario il PID del processo in attesa.
2.  La funzione è necessaria per implementare la system call `getpid` e sarà chiamata direttamente da un processo utente.
3.  La funzione è necessaria per implementare la system call `getpid` e sarà chiamata dal kernel, nella funzione `syscall`.
4.  La funzione restituisce il PID del processo corrente.
5.  La funzione viene chiamata per ottenere il PID di un altro processo (quello da attendere per la terminazione).
6.  La funzione viene chiamata da `sys_exit`, per conoscere il PID del processo da segnalare.

B) Viene fornita una versione della funzione `proc_wait`, presente nella soluzione proposta nel lab 4. La funzione contiene 3 errori. Trovarli e, per ciascuno di essi, fornire una spiegazione (perché sono errori?).

```c
int proc_wait (struct proc *proc)
{
    int return_status;

    KASSERT (proc != NULL);
    KASSERT (proc != kproc);

    cv_wait(proc->p_cv, proc->p_lock);
    lock_acquire(proc->p_lock);

    return_status = proc->p_status;
    if (return_status < 0)
        return return_status;

    lock_release(proc->p_lock);
    proc_destroy (curproc);

    return return_status;
}
```


---

**Risposte**

A.
1.  La funzione è necessaria per implementare `sys_waitpid`, perché è necessario il PID del processo in attesa. **Falso**:
    * `sys_waitpid`ha bisogno del PID del figlio (quello da attendere), non del PID del padre (cioè di chi chiama la wait).
    * `sys_getpid` restituisce il PID del processo corrente, non serve per implementare la logica della wait su un altro processo. 
2.  La funzione è necessaria per implementare la system call `getpid` e sarà chiamata direttamente da un processo utente. **Falso**:
    * I processi utente invocano la system call `getpid`, ma non chiamano direttamente la funzione kernel `sys_getpid`;
    * La chiamata user→kernel avviene tramite un "trap": il processo utente chiama la system call, il kernel esegue la funzione `sys_getpid`;
3.  La funzione è necessaria per implementare la system call `getpid` e sarà chiamata dal kernel, nella funzione `syscall`. **Vero**:
    * Quando un processo utente chiama `getpid`, viene generata una trap e il kernel, tramite il dispatcher delle syscall, chiama `sys_getpid` per restituire il valore.
4.  La funzione restituisce il PID del processo corrente. **Vero**:
    * `sys_getpid` accede a `curproc->pid` e restituisce il PID del processo che sta attualmente eseguendo;
5.  La funzione viene chiamata per ottenere il PID di un altro processo (quello da attendere per la terminazione). **Falso**:
    * Per attendere un altro processo, il PID viene passato come argomento (`waitpid(pid,...)`), non ottenuto tramite `sys_getpid`;
6.  La funzione viene chiamata da `sys_exit`, per conoscere il PID del processo da segnalare. **Falso**:
    * `sys_exit` si occupa di terminare il processo corrente, il PID è già noto tramite `curproc` e non serve chiamare `sys_getpid`;

B.
<table>
  <thead>
    <tr>
      <th>#</th>
      <th>Errore</th>
      <th>Spiegazione</th>
    </tr>
  </thead>
  <tbody>
    <tr>
    <th>1</th>
      <td>`cv_wait(proc->p_cv, proc->p_lock);` viene chiamata senza possedere il lock.</td>
      <td>Prima di chiamare cv_wait, bisogna acquisire il lock associato alla condition variable, altrimenti il comportamento è indefinito e può causare race condition.</td>
    </tr>
    <tr>
    <th>2</th>
      <td>Il return anticipato (prima di lock_release) in caso di errore di status restituisce senza rilasciare il lock e senza chiamare proc_destroy</td>
      <td>Ritornare mentre si detiene un lock può causare deadlock. Inoltre, si rischia di non chiamare le cleanup necessarie (proc_destroy), lasciando risorse non liberate.</td>
    </tr>
    <tr>
<th>3</th>
      <td>proc_destroy(curproc); invece di proc_destroy(proc);</td>
      <td>La funzione deve distruggere il processo passato come argomento (il figlio che ha terminato), non il processo corrente (che è il padre in attesa): distruggere il processo corrente porterebbe alla terminazione errata del padre invece che alla cleanup del figlio.</td>
    </tr>
  </tbody>
</table>