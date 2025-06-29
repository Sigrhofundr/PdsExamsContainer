### Esercizio 4 - Implementazione della `read()` in OS161

Considera una possibile implementazione della `read()` in OS161.

A) La destinazione della lettura può trovarsi nella memoria del kernel, o deve essere (sempre) nella memoria utente?

B) Supponiamo che due processi utente, in esecuzione concorrente, aprano lo stesso file in lettura e chiamino `read(fd, v, N)` su quel file. Ci si aspetta che (per ogni opzione rispondi SÌ/NO e motiva)

1. `fd` sia lo stesso (numero intero) per entrambi i processi.
2. I due processi leggano il file in modo indipendente l'uno dall'altro (o condividono lo stesso flusso/sequenza di lettura?).
3. Sia necessario un lock, perché le operazioni devono essere eseguite in mutua esclusione.
4. Se l'implementazione della system call `read` utilizza un buffer del kernel, i due processi useranno lo stesso buffer.

C) Data la seguente implementazione di `sys_read` per le operazioni di lettura su un file:

```c
static int file_read(int fd, userptr_t buf_ptr, size_t size) {
    ...
    kbuf = kmalloc(size);
    uio_kinit(&iov, &ku, kbuf, size, of->offset, UIO_READ);
    result = VOP_READ(vn, &ku);
    if (result) {
        return result;
    }
    of->offset = ku.uio_offset;
    nread = size - ku.uio_resid;
    copyout(kbuf, buf_ptr, nread);
    kfree(kbuf);
    return (nread);
}
```
Rispondi alle seguenti domande:

1. L'operazione sfrutta un buffer del kernel? (motiva)
2. È possibile che il valore restituito sia diverso dal valore del parametro size? (motiva)
3. `kmalloc` potrebbe essere chiamato con un parametro < size (ad esempio, `kmalloc(size/2)`)? (motiva)

---

---
>[!WARNING]
> Soluzione presa direttamente dal report del prof in quanto non l'avevo in programma
---

**Risposte**

A) Deve essere nella memoria utente, poiché si tratta di una system call destinata ai processi utente.

B)
1. NO. I processi possono avere numeri di fd diversi. Il descrittore di file dipende dalla sequenza di chiamate open eseguite da un processo.
2. SÌ. Le letture sono indipendenti perché ogni processo ha effettuato una chiamata open sul file, e quindi ha la sua propria struttura openfile.
3. NO. Non è necessario un lock perché stanno solo eseguendo operazioni di lettura.
4. Dipende dall'implementazione. Se il buffer del kernel viene allocato e liberato dinamicamente all'interno della system call, i buffer non sono condivisi. 
Il buffer potrebbe essere condiviso solo se implementato come un buffer globale/unico, ma in tal caso questa soluzione richiederebbe una corretta sincronizzazione (mutua esclusione).

C)
1. SÌ. kbuf punta a un buffer del kernel.
2. SÌ. size è il numero massimo di byte da leggere, ma il file potrebbe terminare prima di raggiungere tale numero.
3. SÌ. Tuttavia, ciò richiederebbe multiple operazioni VOP_READ per leggere tutti i size byte. L'implementazione proposta legge tutti i byte in una singola chiamata VOP_READ, quindi se manteniamo l'implementazione attuale, la risposta è NO.