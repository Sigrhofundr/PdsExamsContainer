### Domanda 3 - Gestione dello spazio libero

Considera l'uso di liste concatenate e bitmap per la gestione dello spazio libero, sia per i frame liberi (nella gestione della memoria) che per i blocchi liberi (nei file system).
Rispondi sì/no (vero/falso) alle seguenti affermazioni e fornisci la motivazione per la risposta.

A) - Le bitmap sono migliori delle liste concatenate quando si cercano intervalli di frame/blocchi contigui.<br>
B) - In un sistema con paginazione, le liste di spazio libero sono la soluzione migliore sia per la memoria del processo che per la memoria del kernel.<br>
C) - I blocchi liberi per un file system basato su i-node sono meglio gestiti con le bitmap.<br>
D) - La ricerca di un blocco libero in una lista di spazio libero (concatenata) ha un costo lineare
(rispetto alla dimensione della lista) con la gestione della memoria basata su una tabella delle pagine a due livelli.<br>
E) - Una lista di spazio libero concatenata e ordinata ha un costo lineare (rispetto alla dimensione della lista)
sia per l'allocazione che per la liberazione di spazio contiguo.<br>
F) - Una bitmap, sebbene richieda memoria aggiuntiva, offre prestazioni temporali migliori (rispetto a una
lista di spazio libero) con un file system basato su FAT (File Allocation Table).<br>

---

**Risposte**

A) - Le bitmap sono migliori delle liste concatenate quando si cercano intervalli di frame/blocchi contigui.<br>
**R** - Vero. Le bitmap sono generalmente migliori delle liste concatenate per trovare intervalli contigui perché
permettono algoritmi di ricerca più efficienti (complessità logaritmica vs lineare) e offrono una vista immediata della frammentazione.<br>
B) - In un sistema con paginazione, le liste di spazio libero sono la soluzione migliore sia per la memoria del processo che per la memoria del kernel.<br>
**R** - No. Rappresentano la migliore soluzione per i processi, ma non per il kernel, il quale ha bisogno dell'allocazione della memoria contigua
(per esempio per le tabelle di allocazione delle pagine), mentre i processi non ne hanno bisogno (e le linked list hanno
complessità O(1))<br>
C) - I blocchi liberi per un file system basato su i-node sono meglio gestiti con le bitmap.<br>
**R** - Falso. I file system i-node non richiedono blocchi contigui, quindi le liste concatenate offrono allocazione/liberazione
O(1), mentre le bitmap hanno complessità maggiore anche negli algoritmi ottimizzati. Le liste sono più efficienti
quando la contiguità non è necessaria.<br>
D) - La ricerca di un blocco libero in una lista di spazio libero (concatenata) ha un costo lineare
(rispetto alla dimensione della lista) con la gestione della memoria basata su una tabella delle pagine a due livelli.<br>
**R** - No. Le tabelle delle pagine non hanno bisogno di memoria contigua, quindi le liste hanno complessita O(1);<br>
E) - Una lista di spazio libero concatenata e ordinata ha un costo lineare (rispetto alla dimensione della lista)
sia per l'allocazione che per la liberazione di spazio contiguo.<br>
**R** - Sì. Perché liberare la memoria implica un operazione di inserimento in una lista ordinata, e un allocazione richiede
un operazione di ricerca sulla lista: entrambe le operazioni sono lineari;<br>
F) - Una bitmap, sebbene richieda memoria aggiuntiva, offre prestazioni temporali migliori (rispetto a una
lista di spazio libero) con un file system basato su FAT (File Allocation Table).<br>
**R** - No. Perché la FAT non ha bisogno dell'allocazione contigua quindi una free list (direttamente implementata nella FAT)
è O(1);