## Domanda 1
Nel contesto della gestione della memoria virtuale, considera un sistema con demand paging. Rispondi alle seguenti domande:

A - La politica di sostituzione delle pagine del working set è una politica a dimensione fissa? (SI/NO) Spiega/motiva brevemente.
**R** - No. La dimensione del resident set (ovvero il working set) cambia a seconda dell'insieme di pagine incluse nella finestra Delta.

B - Perché è difficile implementare la politica del working set? (scegli la risposta/e giusta/e, sono possibili più risposte)<br>
    - Perché è difficile indovinare un buon tempo delta.<br>
    **R** - No. Il valore di delta influisce solo sulle performance, non sulla fattibilità della policy;<br>
    - Perché il resident set dovrebbe essere aggiornato anche in caso di assenza di PF.<br>
    **R** - Sì. In ogni momento il sistema deve monitorare costantemente quali pagine vengono accedute,
rimuovere dal resident set le pagine che non sono state accedute per un tempo superiore a Δ, ecc. ;<br>
    - Perché la tecnica deve tenere traccia dell'ultimo tempo di accesso per ogni pagina.<br>
    **R** - Sì, come detto nella risposta precedente, le pagine il cui ultimo accesso è superiore a t-Δ devono essere rimosse;<br>
    - Perché la tecnica richiede di mantenere una lista dei tempi di accesso per ogni pagina.<br>
     **R** - No. La lista completa dei tempi di accesso sarebbe ridondante, basta il tempo dell'ultimo accesso.<br>

C - Considera una strategia LRU basata su un'implementazione a stack (con 5 frame). Data la seguente stringa di riferimento:
      4, 6, 4, 1, 7, 8, 2, 2, 3 (T1), 4, 2 (T2).
      Mostra lo stack (la lista) al tempo T1 (dopo aver acceduto alla pagina 3) e T2 (dopo aver acceduto alla pagina 2).
      (rappresenta lo stack con numeri separati da virgole, con la cima dello stack per prima (es. 1,2,3,4,5 significa che 1 è la cima dello stack))

**R** - LRU - Least Recently Used - rimuoveremo la pagina usata da più tempo dopo il 5° frame
      Quindi dallo stack vuoto avremo: - 4, inserimento - 6, inserimento in cima, - 4, spostato in cima, - 1, inserimento in cima, - 7, inserimento in cima, - 8, inserimento in cima,
        - 2, inserimento in cima e rimozione del 6, - 2 rimane in cima, -3 inserimento in cima e rimozione di 4
          a **T1** avremo `[3,2,8,7,1]`
        - 4, inserimento in cima ed esce 1 - 2, passa in cima
          a **T2** avremo `[2,4,3,8,7]`

D - Considera due stringhe di riferimento w1 e w2 con la stessa lunghezza.
      Sappiamo che $p_2=2 \times p_1$ (probabilità delle stringhe). Due algoritmi di sostituzione di pagina A1 e A2 sono valutati
      _usando lo stesso valore di m (fisso)_. A1 produce lo stesso numero di page fault ($F_1=F_2=5000$) con w1 e w2.
      Nel caso di A2, $F_1$ e $F_2$ cambiano, ma la loro somma rimane invariata. Poiché la frequenza di PF con A1 è del 20% peggiore
      di A2, A2 viene infine selezionato. Calcola i valori di $F_1$ e $F_2$ misurati con l'algoritmo A2.
      (m è omesso nelle formule in quanto costante/invariato in tutti gli esperimenti).
      
**R** - Dati:
        * $F(A_i)$: frequenza media di page fault per l'algoritmo $A_i$;
        * $F_1(A_i,w_1)$, $F_2(A_i,w_2)$: page fault dell'algoritmo $A_i$ sulle stringhe $w_1$ e $w_2$
        * Probabilità: $p_1 = 1/3$, $p_2 = 2/3$ (da $p_2 = 2 \times p_1$ e $p_1 + p_2 = 1$)
          $F(A_i) = p_1 \times F_1(A_i,w_1) + p_2 \times F_2(A_i,w_2)$
          $F_1$ e $F_2$ sono date per l'algoritmo 1 (A1). Useremo le variabili x e y per $F_1$ e $F_2$ in A2.
          Noi sappiamo che $F(A_1)/F(A_2) = 1.2 \Rightarrow x + y = 10000$
          $F(A_1)/F(A_2) = p_1 \times (5000+2 \times 5000) / p_1 \times (x+2y) = 1.2$
          $1.2 \times (x+2y) = 15000$
          $x+2y = 12500$
          utilizzando la seconda equazione ($x+y = 10000$) porta a $x=7500$, $y=2500$