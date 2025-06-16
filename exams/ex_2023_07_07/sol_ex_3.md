## Domanda 3: Organizzazione e Formati di File

Un'applicazione ha bisogno di organizzare un file con un'intestazione e più sezioni. L'intestazione include, tra gli altri dati, offset e dimensioni di tutte le sezioni nel file.<br>
A) per ciascuna delle domande seguenti, rispondi sì/no e motiva la risposta:
1.  **D** - L'applicazione può assumere che l'intestazione, così come ogni sezione, sia allineata con (memorizzata a partire da) i confini dei blocchi disco nel file system?<br>
    **R** - No. Il livello applicativo non ha visibilità di come il fs gestisca i file e le policy di allocazione. Saranno trasparenti per il corretto funzionamento dell'app, salvo rari casi in cui il file sia di un formato standard specifico, supportato e gestito dallo strato del File Organization Module.
2.  **D** - L'applicazione può organizzare i dati in più file (invece di un singolo file): un file per l'intestazione e uno per ogni sezione? (Se NO, motivare, se SÌ, motivare e spiegare come gli offset delle sezioni nell'intestazione dovrebbero essere rappresentati, codificati o modificati)<br>
    **R** - Sì. è possibile organizzare i dati in più file. Gli offset tradizionali devono essere sostituiti con un sistema che identifichi quale file contiene la sezione e dove trovare i dati in quel file specifico.
    Le strategie principali includono l'uso di identificatori di file paired con offset locali, nomi di file, o eliminazione completa degli offset se ogni sezione occupa l'intero file corrispondente.<br>

B) Un file di testo viene copiato su tre diversi file system, i cui formati di file sono, rispettivamente:
* allocazione contigua (per multipli di un blocco disco)
* allocazione concatenata (senza FAT)
* Inode

Rispondi alla seguente domanda e motiva la risposta:
- **D** - La dimensione di un blocco disco è la stessa su tutti e tre i file system. Possiamo assumere che la frammentazione interna sarà la stessa su tutti i file system?<br>
  **R** - NO. Nell'allocazione concatenata (linked), un blocco dati può contenere meno byte rispetto all'allocazione contigua e i-node, perché deve riservare spazio per il puntatore al blocco successivo. Questo altera il numero di blocchi necessari e quindi la frammentazione interna.

C) Un file system contiene tre file di testo `a.txt`, `b.txt` e `c.txt`. Un'applicazione di archiviazione/compressione file (come ad esempio gzip, tar, 7z, rar, etc.) memorizza il (contenuto di) i tre file di testo all'interno di un singolo file archivio (e.g. `abc.zip`).
<br>Per ciascuna delle domande seguenti, rispondi sì/no e motiva la risposta:
1.  **D** - I tre file di testo possono condividere lo stesso blocco disco, il che significa che il file system li sta memorizzando (almeno parzialmente) all'interno dello stesso blocco disco?<br>
    **R** - No. Perché dato che sono tre file diversi, allo stesso modo il fs alloca ognuno di loro in un set di blocchi disco dedicato.Lo spazio inutilizzato nell'ultimo blocco (frammentazione interna) rimane riservato esclusivamente a quel file e non può essere utilizzato da altri file. L'isolamento tra file è un principio fondamentale che impedisce la condivisione di blocchi, garantendo integrità e sicurezza dei dati.
2.  **D** - È possibile, per il contenuto dei tre file di testo, replicato e/o compresso nel file archivio, condividere (almeno parzialmente) lo stesso blocco disco (so to be stored within the same disk block)?<br>
    **R** - Sì. Perché l'archivio è un singolo file, e dunque, anche se non è garantito, è possibile che i byte provenienti dai diversi file convergano nello stesso blocco su disco.
