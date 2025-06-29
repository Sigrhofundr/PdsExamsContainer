### Domanda 5

Considera una possibile implementazione delle chiamate di sistema `open()` e `close()` in OS161.

A) La tabella per processo si trova nella memoria utente o nella memoria kernel?
B) Supponiamo che due processi utente, in esecuzione concorrente, chiamino `write(fd,v,N)`, dove `fd` è 5 per entrambi, ti aspetti che (per ogni opzione rispondi SÌ/NO e motiva):
1. i processi scrivano sullo stesso file
2. non scrivano mai sullo stesso file
3. generalmente scrivano su file diversi ma a volte possano scrivere sullo stesso
4. per scrivere sullo stesso file, i due processi dovrebbero usare valori diversi per `fd`.

C) Data la seguente implementazione di `sys_write`:

```c
int sys_write (int fd, userptr_t buf_ptr, size_t size) {
    int i;
    char *p = (char *)buf_ptr;

    if (fd!=STDOUT_FILENO && fd!=STDERR_FILENO) {
        return file_write(fd, buf_ptr, size);
    }

    for (i=0; i<(int)size; i++) {
        putch (p[i]);
    }

    return (int)size;
}
```

Assumendo che input, output ed errore standard siano mappati sulla console (quindi non è possibile la ridirezione su file), il ciclo `for` che gestisce `stdout`/`stderr`
può essere sostituito da uno (o più) dei seguenti frammenti di codice? (Per ciascuno di essi rispondi sì/no e fornisci una motivazione).

1.
```c
for (i=0; i<(int)size; i++) {
    kprintf("%c", p[i]);
}
```
2.
```c
kprintf("%s", p);
```
3.
```c
kprintf("%s", &p);
```
4.
```c
file_write(fd, p, size);
```
---

**Risposte**

> >[!WARNING]
> Soluzione presa direttamente dal report del prof in quanto non l'avevo in programma

A) Si trova nella memoria del kernel. La tabella è una struttura dati interna del kernel: il fatto di essere dedicata a un dato processo non ha nulla a che fare con la sua accessibilità in modalità utente per quel processo.<br>
B) Considerazioni preliminari: i descrittori di file vengono assegnati su base per-processo e, a eccezione dell'input/output/errore standard (ma il punto 5 esclude questo caso), 
ogni volta che due processi aprono un dato file, anche se il file è lo stesso, nulla vincola la funzione open a restituire lo stesso descrittore di file. 
Di conseguenza, quando due processi ottengono lo stesso numero per un descrittore di file, non è detto che il file sia lo stesso.
1. NO. I file possono essere diversi.
2. NO. Sebbene non sia frequente, i descrittori di file potrebbero essere mappati sullo stesso file (ovviamente sarebbe necessario un qualche tipo di lock).
3. SÌ. Stessa spiegazione della risposta precedente.
4. NO. Come spiegato nelle considerazioni preliminari, non ci sono vincoli sui descrittori di file.

C)
1. SÌ. Perché `kprintf`, con il formato `%c`, è equivalente a `putch`.
2. NO. Sebbene l'istruzione C sia corretta, il formato %s richiede una stringa terminata da `'\0'`, cosa che non può essere garantita dalla system call write.
3. NO. Oltre all'osservazione precedente sulla terminazione della stringa, l'istruzione è formalmente errata, poiché `&p` è di tipo `char**`.
4. NO. Perché la funzione `file_write` supporta solo l'output basato su file, non la console. Se `file_write` supportasse l'output su console, il caso speciale per la gestione della console non avrebbe senso in `sys_write`.