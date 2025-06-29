## Domanda 4: Framework del File System di OS161 (LAB5)

Viene dato un sistema OS161. Considera il framework del file system utilizzato nel LAB5, parzialmente rappresentato dai seguenti estratti di codice:

```c
// Code excerpts from files proc.h and file_syscalls.c

struct proc {
    // ...
    #if OPT_FILE
    /* per-process open file table */
    struct openfile *fileTable[OPEN_MAX];
    #endif
};

/* system open file table */
struct openfile {
    struct vnode *vn;
    off_t offset;
    unsigned int countRef;
};
struct openfile systemFileTable[SYSTEM_OPEN_MAX];
```

A) Perché `systemFileTable` è un array di `struct openfile`, mentre `fileTable` è un array di puntatori a `struct openfile`?
B) Il campo `countRef` è ridondante, considerando che il `vnode` puntato dal campo `vn` ha il proprio conteggio di riferimenti?
C) Nel caso in cui sia necessario supportare il blocco dei file (file locking), cosa dovremmo scegliere tra uno spinlock e un lock, e dove dovrebbe essere posizionato (o dove dovrebbero essere posizionati, se multipli): in `systemFileTable`, in `fileTable`, o altrove?
D) Qual è il possibile ruolo delle funzioni `copyin` e `copyout` nell'implementazione delle chiamate di sistema `sys_read` e `sys_write`?

---

> >[!WARNING]
> Soluzione presa direttamente dal report del prof in quanto non l'avevo in programma

***Risposte**

A) Una `struct openfile` è una struttura dati associata creata all'apertura di un file: ogni `struct openfile` contiene un offset per la lettura all'interno di un file. 
Due processi che condividono l'offset durante la lettura/scrittura condivideranno la `struct openfile`, mentre se usano offset indipendenti, useranno `struct openfile` diverse. 
Per consentire la condivisione (o meno) delle `struct openfile`, queste vengono allocate nella tabella a livello di sistema (system-wide table). <br>
I processi possono solo puntare alle `struct openfile`, in modo da poterle condividere o meno.

B) No, non è ridondante. Il conteggio dei riferimenti del `vnode` conta quante `vfs_open` sono state eseguite (cioè quanti offset sono attivi sul file), mentre `countRef` nella `struct openfile` 
conta quanti processi stanno condividendo l'offset (e la struct `openfile` che lo contiene).

C) A meno che non si opti per una reimplementazione a basso livello di un lock, il lock dovrebbe essere preferito agli spinlock, perché le operazioni sui file 
non sono abbastanza veloci da giustificare il busy waiting. Il lock deve essere accessibile/utilizzato da tutti i processi che lavorano su un dato file. 
Poiché un lock è una struttura dati allocata dinamicamente, è sufficiente crearlo una sola volta: i puntatori al lock potrebbero trovarsi nella `struct openfile` o 
nella tabella per-processo, a seconda dello schema di blocco adottato.

D) `copyout()` e `copyin()` sono utilizzate per copiare i dati in modo sicuro tra la memoria utente e la memoria del kernel, ovvero senza che il kernel vada in crash 
in caso di puntatori utente non validi. Possono essere utilizzate nell'implementazione di `sys_read` e `sys_write`, a condizione che l'I/O sia eseguito tramite buffer del kernel.