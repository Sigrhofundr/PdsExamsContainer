## Domanda 3

Considera la gestione della memoria con paging e una MMU con TLB. Per ciascuna delle seguenti domande, rispondi SÌ o NO e fornisci una motivazione/spiegazione:
* **D** - La reach della TLB diminuisce quando la dimensione della pagina aumenta?
**R** - No, aumenta, in quanto il numero di record all'interno della TLB è costante. Per definizione, la reach della TLB è la quantità totale di memoria virtuale
che può essere indirizzata attraverso le entry presenti nella TLB. Quindi $Reach_{TLB} = Numero_{entry\_TLB} \times Dimensione_{pagina} \Rightarrow$
Se aumenta la dimensione, aumenta la reach.

    * **D** - La frammentazione aumenta, quando la dimensione della pagina aumenta, perché sono necessarie partizioni contigue più grandi?
      **R** - No, la frammentazione interna aumenta effettivamente con l'aumentare della dimensione delle pagine,
      ma la ragione proposta è sbagliata. Nel paging non esiste allocazione contigua - le pagine virtuali possono essere
      mappate su frame fisici qualsiasi, quindi non sono necessarie "partizioni contigue più grandi". La frammentazione interna
      aumenta perché lo spreco medio nell'ultima pagina è maggiore;

    * **D** - Il prepaging è utile solo se la probabilità che una pagina prepaginata venga effettivamente utilizzata è > 80%?
      **R** - No, è utile comunque, basta una probabilità significativamente alta (solitamente al di sopra del 50%).
      Una soglia dell'80% è troppo restrittiva e non ha basi teoriche solide.

    * **D** - Tutte le strutture dati del kernel richiedono allocazione contigua?
      **R** - No, solo le parti del kernel le cui strutture dati hanno bisogno di allocazione continua (per efficienza):
      ad esempio 1. Buffer I/O e DMA; 2. Strutture hardware-sensitive (i.e. la tabella delle pagine); 3.
      Strutture piccole e performance-critical (i.e. Process control blocks (PCB))

    * **D** - L'allocatore slab utilizza solo dimensioni potenza di 2?
      **R** - No, è il sistema buddy che comporta un allocatore a potenze di 2;

    * **D** - Una free list di pagine ha una frammentazione interna media di mezza pagina?
      **R** - No, essendo una lista di pagine libere non ha problemi di frammentazione interna in quanto non contiene dati, ma rappresentano
      lo spazio disponibile