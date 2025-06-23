## Domanda 3

Si considerino le ottimizzazioni implementate per migliorare le prestazioni complessive della gestione della memoria virtuale.

A. Spiegare brevemente la tecnica COW (Copy on Write): che cosa è e come migliora le prestazioni della gestione della memoria? 
I miglioramenti sono in termini di tempo? Di occupazione di memoria? Di entrambi tempo e memoria? (Motivare)

B. Spiegare perché l'IO sulla partizione di swap (backing store) è più veloce dell'IO mediante file system (anche se a parità di disco). 
È possibile che lo spazio di indirizzamento virtuale di un processo sia più grande del numero di blocchi totali disponibili su backing store? (Motivare)

C. Quale è il ruolo del bit di validità (valid/invalid bit) associato a una pagina? 
Il validity bit viene memorizzato solo nella tabella delle pagine oppure viene anche immagazzinato nella TLB (quando disponibile su un dato processore)? (Motivare)

---
**Risposte**

A. CoW è un'ottimizzazione della gestione memoria che ritarda la copia fisica delle pagine fino al momento della modifica.
Sfrutta il principio di località temporale: molte pagine condivise potrebbero non essere mai modificate.
Il beneficio è duplice: risparmio memoria (condivisione prolungata) e risparmio tempo (eliminazione copie inutili).
La protezione hardware delle pagine (read-only) implementa il trigger per la copia differita.

B. Lo swap è più veloce perché elimina l'overhead del filesystem: accesso diretto ai blocchi tramite offset matematici, 
nessuna gestione di metadata/inode, layout lineare ottimizzato. 
Il filesystem richiede traduzioni complesse nome→inode→blocchi e mantiene strutture dati aggiuntive.
Lo spazio virtuale può superare il backing store perché è teorico, non fisico. 
Il kernel usa overcommit: alloca virtualmente più memoria di quella disponibile, sfruttando la località temporale. 
Non tutte le pagine virtuali sono simultaneamente attive, quindi il backing store serve solo per le pagine effettivamente swappate.

C. Il bit di validità (valid/invalid bit) associato a ogni pagina indica se la voce della tabella delle pagine punta a una pagina di memoria fisica effettivamente valida e accessibile dal processo.
* Se il bit è valido (1), la pagina richiesta si trova in RAM (o comunque è presente e valida).
* Se il bit è non valido (0), la pagina non è attualmente in memoria (page fault, quindi va caricata dal disco), oppure l’indirizzo richiesto non appartiene a uno spazio indirizzabile dal processo (accesso non autorizzato, generando un’eccezione).

Il bit di validità è presente sia nella tabella delle pagine che nella TLB:
* Nella tabella delle pagine: serve al sistema operativo per sapere se la mappatura logico-fisica è attiva e valida.
* Nella TLB (Translation Lookaside Buffer): quando una voce della page table viene caricata nella TLB, il bit di validità viene copiato anche nella TLB. Questo permette al processore di verificare rapidamente, senza accedere alla page table, se la traduzione è valida.
