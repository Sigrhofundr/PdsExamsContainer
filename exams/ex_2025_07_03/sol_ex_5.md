## Domanda 5 (comprende le domande da 19 a 20 del questionario) - Risposte con Sì/No alle affermazioni/domande

### Domanda 19
//TODO da verificare una volta pubblicate le soluzioni
_Premessa:_
Consideriamo le funzioni `thread_switch` e `thread_yield`.

_Domande:_
* `thead_switch` è più generale, poiché `thread_yield` è implementato dalla chiamata `thread_switch(S_READY, NULL, NULL);` 
**Sì** - `thread_switch` è la funzione di basso livello che realizza il cambio di contesto vero e proprio tra thread; `thread_yield` è una sua particolare applicazione 
(il thread corrente si rimette in coda come pronto). Quindi `thread_yield` si basa su `thread_switch` e quest’ultima è più generale.
* `thread_switch` lavora sulla ready queue, rimuovendo dalla coda un thread pronto per l'esecuzione; **Sì**
* `thread_switch` chiama `switchframe_switch(&cur->t_context, &next->t_context);` in cui i registri della CPU vengono
  salvati in `next->t_context` e ripristinati da `cur->t_context`; **No** - è il contrario
* Il prototipo di `thread_yield` è `void thread_yield(void)`, quindi la funzione può essere utilizzata solo per interrompere
  l'esecuzione del thread corrente (quindi nessun altro thread) e metterlo nello stato `S_READY` 
**Sì** - thread_yield può essere chiamata solo dal thread corrente, che volontariamente cede la CPU e si rimette nello stato pronto (S_READY).
Non può essere usata per agire su altri thread.
* `thread_switch` viene chiamato da `wchan_sleep`, per mettere un thread nello stato `S_SLEEP` (che significa attesa) **Sì**

---

### Domanda 20

_Premessa:_
Considerare le funzioni `thread_create` e `thread_fork`

_Domande:_
* `thread_create` alloca e inizializza semplicemente la struttura C del thread 
**Sì** - La funzione `thread_create` in OS/161 è responsabile di allocare la memoria per una nuova struttura thread 
(spesso chiamata struct thread) e di inizializzarne i campi fondamentali. Questi campi includono l'identificatore del thread, 
lo stato iniziale (solitamente `S_READY`), e un puntatore al suo stack del kernel. Tuttavia, `thread_create` non si occupa 
di avviare l'esecuzione del thread o di schedularlo; si limita a preparare la sua struttura dati in memoria.
* `thread_fork` chiama `thread_create` 
**Sì** - `thread_fork` è una funzione di livello superiore rispetto a `thread_create` . Il suo scopo è creare un nuovo thread e 
prepararlo per l'esecuzione. Per fare ciò, `thread_fork` internamente chiama `thread_create`  per ottenere una nuova struttura 
thread allocata e inizializzata. Successivamente, `thread_fork` prosegue con ulteriori passaggi, come la configurazione 
del contesto di esecuzione del nuovo thread e l'aggiunta alla ready queue in modo che lo scheduler possa eseguirlo.
* `thread_fork` genera un processo utente figlio/child 
**No** - `thread_fork` crea un nuovo thread (cioè un flusso di esecuzione), non un nuovo processo. In OS161, la creazione 
di un nuovo processo (struct proc) avviene con altre funzioni
* `thread_fork` riceve come parametro (un puntatore a) una funzione, da richiamare in caso di errore (se, ad esempio, il thread non può essere creato). 
**No** - `thread_fork` riceve come parametro un puntatore a funzione che rappresenta la routine che il nuovo thread 
dovrà eseguire, non una funzione di errore. In caso di errore restituisce un codice di errore, ma non chiama una callback.
* `thread_fork` ha un parametro struct proc * proc, che può essere NULL o un puntatore a una struct proc esistente **Sì**