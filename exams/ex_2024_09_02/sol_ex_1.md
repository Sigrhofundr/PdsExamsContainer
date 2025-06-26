## Domanda 1

_(SÌ/NO, Spiega/motiva brevemente)_

1. In un sistema con tabella delle pagine invertita, una singola entry della tabella delle pagine può essere
   condivisa da più pagine virtuali di processi diversi senza causare un conflitto nella traduzione degli indirizzi?
2. In un sistema che utilizza la sostituzione di pagina locale, un processo con un tasso molto elevato di
   page fault può comunque causare una riduzione della memoria fisica disponibile ad altri processi?
3. In un sistema di memoria virtuale con un grado di multiprogrammazione fisso, il thrashing può comunque
   verificarsi se la somma dei working set di tutti i processi è inferiore alla memoria fisica disponibile ma l'algoritmo
   di sostituzione delle pagine non è ottimizzato?
4. Considera un sistema a 64 bit con uno spazio di indirizzamento virtuale di $2^{48}$ byte, una dimensione di
   pagina di 4 KB e 16 GB di memoria fisica. Supponiamo che il sistema utilizzi una tabella delle pagine invertita.
   Calcola e spiega quanto segue:<br>
   a. Il numero di bit richiesti per il numero di frame fisico.<br>
   b. Il numero totale di entry nella tabella delle pagine invertita.<br>
   c. L'indirizzo virtuale dato a un processo è $0x00007FFFFFFFF000$. Determina l'indirizzo fisico
   se questo indirizzo virtuale è mappato al 1.024° frame fisico. Mostra tutti i passaggi,
   incluso il calcolo del numero di pagina e dell'offset.

---

### Risposte
1. **Sì** - (in realtà dipende), nella forma di base della IPT, nel quale ogni entry detiene il numero di pagina e il pid, non sarebbe possibile,
   in quanto un dato frame può appartenere a un solo processo. Questo sarebbe un problema per le pagine/librerie condivise.
   Dato che solitamente le IPT sono integrate con le tabelle hash, la maggior parte delle IPT contengono anche un puntatore, utilizzato per
   le collisioni hash di catene/liste. La tabella delle pagine di tipo hash standard non sarebbe abbastanza per condividere un
   IPT entry a seconda dei processi diversi (a causa di come la funzione di hash funziona, le entries non sarebbero nello
   stessa lista di collisioni dell'hash). Può essere fatto grazie a delle soluzioni software sofisticate, nel quale
   si usa il puntatore IPT, nel caso di frame condivisi, per una lista di pagine che condividono il frame.
2. **No**, nella sostituzione locale pura ogni processo ha un resident set fisso e può sostituire solo le
   proprie pagine, quindi non può direttamente sottrarre memoria fisica ad altri processi.
3. **Sì**, il trashing accade quando l'algoritmo di sostituzione di pagina non predice correttamente o gestisce
   correttamente il working set dei processi, portando a frequenti e non necessarie sostituzione di pagina. Questa inefficienza
   può causare ai processi la rimozione ripetuta di pagine di cui presto avranno bisogno nuovamente, risultando in un eccessivo numero
   di page fault e trashing, nonostante la memoria fisica sia sufficiente.
4. premessa:
    * Sistema 64 bit; spazio indirizzamento $2^{48}$ (281TB)
    * DIM Pagina 4 KB = $2^{12}$ ; 16 GB = $2^{34}$ memoria<br>
      a. Il numero di frame fisici, RAM / dim pagina = $2^{34}$ / $2^{12}$ = $2^{22}$ =4.194.304 circa 4M, ma da questo indice ci ricaviamo che il numero di bit richiesti
      per il physical frame number è **22**.<br>
      b. Sarà uguale al numero dei frame fisici per definizione: **$2^{22}$**<br>
      c. Virtual Address: $0x00007FFFFFFFF000$; Page size: $2^{12}$, ricordiamoci anche che i bit necessari li otteniamo effettuando la funzione inversa
      rispetto al dato che ci viene dato, quindi $log_24096$ = 12;
    * l'offset, sarà dunque di 12 bit (gli ultimi 12, i 3 0 finali esadecimali)
    * il numero di pagina virtuale sarà dato allora da $0x00007FFFFFFFF$
    * Nella fase successiva, il 1024esimo frame corrisponderà al numero di frame fisico di 1024, in esadecimale $0x00000400$
    * Per ottenere l'indirizzo fisico, combiniamo il physical frame con l'offset
    * Indirizzo_fisico = $(frame << 12) | offset => (0x00000400 << 12) | 0x000$ ==> $0x000400000$ <!--Nella soluzione caricata del prof c'è uno zero extra-->

