## Domanda 3 (comprende le domande da 12 a 14 del questionario) - Risposte con Sì/No alle affermazioni/domande

### Domanda 12

_Premessa:_
Un'applicazione deve organizzare un file con un'intestazione (header) e più sezioni. L'intestazione include, tra
gli altri dati, offset e dimensioni di tutte le sezioni del file.

_Domande:_
* Una volta che conosciamo (dopo averlo letto dall'intestazione) l'offset (nel file) di una sezione, è possibile leggere
  la sezione con accesso diretto/casuale con tutti i formati di allocazione dei file (oppure alcuni formati richiedono l'accesso sequenziale)?
**RISPOSTA REPORT PROF CABODI:** YES
* Se l'intestazione è memorizzata in un file separato, questa deve essere allocata in blocchi di dati contigui? **RISPOSTA REPORT PROF CABODI:** NO
* L'applicazione può presupporre che l'intestazione e ciascuna sezione siano allineate ai (memorizzate a partire dai)
  limiti dei blocchi del disco nel file system? **RISPOSTA REPORT PROF CABODI:** NO
* È obbligatorio che l'applicazione organizzi i dati in più file (invece che in un unico file): un file per l'intestazione e uno per ogni sezione?
  **RISPOSTA REPORT PROF CABODI:** NO
---

### Domanda 13

_Premessa:_
Un file di testo viene copiato in tre diversi file system, i cui formati di file sono, rispettivamente:
- allocazione contigua (per multipli di un blocco del disco, DMA abilitato)
- allocazione basata su linked list (senza FAT), DMA abilitato
- Inode, con DMA disabilitato

_Domande:_
* L'allocazione contigua e l'inode garantiscono l'accesso casuale/diretto a tempo costante $(O(1))$ **RISPOSTA REPORT PROF CABODI:** YES
* La dimensione di un blocco del disco è la stessa su tutti e tre i file system. Possiamo supporre che la frammentazione
  interna sia la stessa su tutti i file system **RISPOSTA REPORT PROF CABODI:** NO
* Durante la lettura dell'intero file dall'inizio alla fine, l'allocazione collegata sarà più efficiente rispetto ad altri
  formati di allocazione (contigui e inode) perché l'elenco collegato è particolarmente efficace con l'accesso sequenziale **RISPOSTA REPORT PROF CABODI:** NO
* Durante la lettura dell'intero file dall'inizio alla fine, l'allocazione basata su linked list sarà probabilmente più
  efficiente dell'inode, in termini di tempo di esecuzione, grazie al DMA. **RISPOSTA REPORT PROF CABODI:** yes

---

### Domanda 14

_Premessa:_
Un'applicazione utilizza la modalità asincrona per leggere un file di dati (un file binario di numeri interi a 32
bit) e calcolare la somma dei numeri.

_Domande:_
* È possibile avviare letture multiple, ma nessun dato deve essere utilizzato finché l'IO corrispondente non è stato completato **RISPOSTA REPORT PROF CABODI:** YES
* Se il file viene letto utilizzando più thread, la somma deve essere considerata come sezione critica (e quindi trattata
  con opportune primitive di sincronizzazione)? **RISPOSTA REPORT PROF CABODI:** YES
* Per garantire che tutti i dati siano stati letti, è sufficiente che l'applicazione attenda il completamento dell'ultimo IO,
  perché quando questo viene fatto, vengono eseguiti anche tutti gli IO precedenti. **RISPOSTA REPORT PROF CABODI:** NO
* La modalità asincrona è implementata internamente dall'I/O basato su interrupt, perché il polling non può supportare
  la modalità asincrona **RISPOSTA REPORT PROF CABODI:** NO

---