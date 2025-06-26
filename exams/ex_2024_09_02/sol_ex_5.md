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
<table>
  <thead>
    <tr>
      <th>#</th>
      <th>Error</th>
      <th>Explain</th>
    </tr>
  </thead>
  <tbody>
    <tr>
    <th>1</th>
      <td></td>
      <td></td>
    </tr>
    <tr>
    <th>2</th>
      <td></td>
      <td></td>
    </tr>
    <tr>
<th>3</th>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>

---

**Risposte**