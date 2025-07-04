## Domanda 5 (comprende le domande da 19 a 20 del questionario) - Risposte con Sì/No alle affermazioni/domande

### Domanda 19

_Premessa:_
Consideriamo le funzioni `thread_switch` e `thread_yield`.

_Domande:_
* `thead_switch` è più generale, poiché `thread_yield` è implementato dalla chiamata `thread_switch(S_READY, NULL, NULL);`
* `thread_switch` lavora sulla ready queue, rimuovendo dalla coda un thread pronto per l'esecuzione;
* `thread_switch` chiama `switchframe_switch(&cur->t_context, &next->t_context);` in cui i registri della CPU vengono
  salvati in `next->t_context` e ripristinati da `cur->t_context`;
* Il prototipo di `thread_yield` è `void thread_yield(void)`, quindi la funzione può essere utilizzata solo per interrompere
  l'esecuzione del thread corrente (quindi nessun altro thread) e metterlo nello stato `S_READY`
* `thread_switch` viene chiamato da `wchan_sleep`, per mettere un thread nello stato `S_SLEEP` (che significa attesa)

---

### Domanda 20

_Premessa:_
Considerare le funzioni `thread_create` e `thread_fork`

_Domande:_
* `thread_create` alloca e inizializza semplicemente la struttura C del thread
* `thread_fork` chiama `thread_create`
* `thread_fork` genera un processo utente figlio/child
* `thread_fork` riceve come parametro (un puntatore a) una funzione, da richiamare in caso di errore (se, ad esempio, il thread non può essere creato).
* `thread_fork` ha un parametro struct proc * proc, che può essere NULL o un puntatore a una
  struct proc esistente