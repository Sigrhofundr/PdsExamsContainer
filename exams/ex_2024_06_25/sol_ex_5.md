## Domanda 5

Considera l'implementazione di lock e variabili di condizione in OS161. Per ciascuna delle seguenti frasi, rispondi SÌ/NO e fornisci una motivazione:

La funzione `cv_wait` riceve un lock come parametro perché:

1. è necessario come parametro nella chiamata interna a `wchan_sleep`
2. il thread chiamante deve essere il proprietario del lock
3. il lock deve essere rilasciato e acquisito di nuovo da `cv_wait`
4. il lock deve essere rilasciato e acquisito di nuovo da `wchan_sleep`

Il lock può essere implementato:

5. da un semaforo binario, senza nessun altro elemento/requisito
6. da un semaforo binario, più un elemento/requisito aggiuntivo
7. da una condition variable
8. da un wait channel

L'implementazione della funzione `lock_acquire` può includere una chiamata a:

9. le funzioni `spinlock_data_get` e `spinlock_data_testandset`
10. la funzione `P` su un semaforo
11. la funzione `cv_wait`
12. la funzione `wchan_sleep`

---

**Risposte**

1. No, la `wchan_sleep` non necessita del lock passato a `cv_wait`, ma richiede invece un spinlock per garantire la mutua esclusione sul canale di attesa. 
Il lock passato a `cv_wait` viene usato per proteggere la variabile di condizione, ma non è direttamente utilizzato da `wchan_sleep`. 
Quest’ultima si occupa solo di gestire la sospensione e il risveglio dei thread su un determinato canale, utilizzando uno spinlock interno.
2. Sì. È fondamentale che il thread che invoca cv_wait sia effettivamente il proprietario del lock passato. 
Questa è una regola di correttezza per l’uso delle variabili di condizione: permette di garantire che la verifica e l’attesa 
sulla condizione siano atomiche rispetto ad altri thread che accedono alla stessa variabile condivisa. Se il thread non possiede il lock, 
l’uso di `cv_wait` non sarebbe sicuro e potrebbe portare a race condition. Inoltre avviene proprio un controllo (`KASSERT(lock_do_i_hold(lock));`) apposito.
3. Sì. La semantica di `cv_wait` prevede che il lock venga rilasciato mentre il thread è in attesa e riacquisito quando il thread viene risvegliato. 
Questo comportamento è essenziale per evitare deadlock e garantire che altri thread possano progredire ed eventualmente modificare 
la condizione che il thread in attesa sta aspettando. Per questo motivo, `cv_wait` ha la responsabilità di gestire il rilascio e la riacquisizione del lock attorno alla chiamata a `wchan_sleep`.
4. No. Il rilascio e la riacquisizione del lock non sono gestiti da `wchan_sleep`, che si occupa solo della gestione dei thread in attesa sul canale associato, 
utilizzando uno spinlock interno per garantire la consistenza del canale stesso. La responsabilità di rilasciare e riacquisire il lock rimane interamente a carico di `cv_wait`.
5. No. Un semaforo binario può gestire l’accesso esclusivo, ma non tiene traccia del proprietario del lock. 
Per un lock, invece, è fondamentale sapere quale thread lo detiene, in modo da impedire, ad esempio, che un thread diverso da quello proprietario chiami `lock_release`. 
Quindi un semaforo binario da solo non è sufficiente.
6. Sì. Se si aggiunge, ad esempio, una variabile che tiene traccia del proprietario del lock (il thread che l’ha acquisito), 
allora il lock può essere implementato su un semaforo binario. L’elemento aggiuntivo serve proprio a garantire che solo il proprietario 
possa rilasciare il lock, mantenendo la semantica corretta.
7. No. Una condition variable non fornisce mutua esclusione; serve solo a sincronizzare l’attesa e il risveglio dei thread su una determinata condizione. 
Una condition variable ha sempre bisogno di un lock esterno per funzionare correttamente e non può essere usata come sostituto di un lock.
8. Sì. Un wait channel, combinato con uno spinlock e una gestione opportuna del proprietario del lock, può essere utilizzato per implementare un lock. 
In OS161, questa è una delle implementazioni canoniche: il canale di attesa gestisce la coda di thread sospesi, mentre lo spinlock assicura la mutua esclusione sulle strutture dati interne del lock.
9. No. Queste funzioni sono tipicamente usate per implementare spinlock a basso livello, non i lock di più alto livello come quelli usati nelle primitive sincronizzazione di OS161. 
Lock come quelli implementati con wait channel si basano su primitive più astratte.
10. Sì. Se il lock è implementato tramite un semaforo binario, la funzione `lock_acquire` farà uso della chiamata a P (operazione di wait sul semaforo) per acquisire il lock, bloccando il thread se il lock non è disponibile.
11. No. La funzione `cv_wait` serve per la sincronizzazione su condizioni, non per la mutua esclusione. Non è adatta a essere usata all’interno di `lock_acquire`.
12. Sì. In un’implementazione basata su wait channel, se il lock non è disponibile, `lock_acquire` può mettere in sleep il thread chiamante su un canale di attesa tramite `wchan_sleep`, 
in attesa che il lock venga rilasciato e reso nuovamente disponibile.
