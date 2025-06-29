### Esercizio 5- Thread in OS161

Dato un sistema OS161.

A) Considera le funzioni `thread_switch` e `thread_yield`. Sono equivalenti o svolgono compiti diversi? (motiva)
B) Quando un nuovo thread del kernel viene creato in OS161, viene eseguito immediatamente o viene messo in coda per l'esecuzione?
C) L'implementazione di `thread_exit()` termina con una chiamata a `switchframe_switch(&cur->t_context, &next->t_context)`.
Che cosa rappresentano i due parametri (non è sufficiente dire "contesto del thread")? Quale dei due parametri si riferisce al contesto salvato e quale a quello ripristinato?

---

**Risposte**

A) Le due funzioni svolgono diversi compiti sui thread. Il `thread_yield` permette al thread corrente di comunicare allo scheduler la cessione del controllo, consentendo così 
la pianificazione e l'esecuzione di altri thread. `thread_switch` prende il contesto del thread da eseguire e lo inserisce nella CPU, salvando il contesto del thread precedente.

B) Non viene eseguito immediatamente, viene messo in coda. Ecco un breve riepilogo del processo di messa in esecuzione del thread. Viene chiamata la `thread_fork` che:
* alloca una nuova struct thread tramite `thread_create`;
* inizializza lo switchframe nello stack dei thread tramite `switchframe_init` che imposta il contesto (registri + PC);
* `thread_make_runnable` prende un thread, e lo aggiunge alla coda dei thread ready, attraverso la funzione `threadlist_addtail`;
* il thread, quando lo scheduler lo deciderò, lo lancerà avviando `thread_switch`, che cambia il contesto del thread da eseguire nella cpu (salvando il precedente);

C) I due parametri di `switchframe_switch(&cur->t_context, &next->t_context)` sono puntatori ai switchframe salvati negli stack kernel dei thread. 
Il primo parametro (`&cur->t_context`) è il punto dove viene salvato il contesto (registri) del thread corrente, mentre il secondo parametro 
(`&next->t_context`) è il punto da cui viene ripristinato il contesto del nuovo thread che prenderà l'esecuzione.
In questo modo, il sistema operativo può sospendere e riprendere l’esecuzione dei thread salvando e caricando i loro stati della CPU.
