## Domanda 1: Caratteristiche del Sistema e Gestione della Memoria

Un sistema informatico presenta le seguenti caratteristiche:
* Dimensione della memoria fisica: 512 MB
* Spazio di indirizzamento virtuale: indirizzi a 32 bit (o 16 bit)
* Dimensione della pagina: 8 KB
* Utilizza una tabella delle pagine a due livelli
* Implementa un algoritmo di sostituzione della pagina Least Recently Used (LRU)

1.  **D** - Una tabella delle pagine può essere progettata per adattare dinamicamente la sua dimensione in base al numero di pagine che un processo sta utilizzando, minimizzando così l'overhead di memoria? (SÌ/NO, Spiegare/motivare brevemente)
    <br>**R** - Sì. Le tabelle delle pagine possono essere progettate per adattarsi dinamicamente, minimizzando l'overhead di memoria attraverso diverse tecniche architetturali. L'implementazione attraverso strutture gerarchiche o hash ottimizza
    l'uso della memoria allocando risorse solo quando effettivamente necessarie, bilanciando efficienza di memoria e prestazioni del sistema.<br>
2.  **D** - Un processo ha allocato più frame fisici rispetto alla dimensione del suo working set (per un dato intervallo di tempo): questo ridurrà sempre a zero il tasso di page fault? Spiegare/motivare brevemente le risposte. (SÌ/NO, Spiegare/motivare brevemente)<br>
    **R** - No. Avere più frame del WS attuale non garantisce zero PF. Il WS è dinamico e cambia durante l'esecuzione del processo, richiedendo pagine diverse nel tempo. Inoltre, la contesa di memoria a livello di sistema può causare sostituzioni di pagine anche quando
    il processo ha frame apparentemente sufficienti per le sue esigenze immediate.<br>
3.  **D** - Due indirizzi virtuali diversi possono mappare allo stesso indirizzo fisico in un sistema che utilizza la paginazione? (SÌ/NO, Spiegare/motivare brevemente)<br>
    **R** - Sì. Questo è possibile attraverso l'utilizzo della memoria condivisa. Differenti processi possono avere differenti indirizzi virtuali che mappano lo stesso indirizzo fisico per la comunicazione tra processori.<br>
4.  **D** - L'uso di una dimensione di pagina più grande può portare a un utilizzo più efficiente della TLB nonostante i potenziali aumenti della frammentazione interna? (SÌ/NO, Spiegare/motivare brevemente)<br>
    **R** - Sì. Una dimensione maggiore delle pagine può aumentare l'efficienza della TLB perché saranno necessari meno righe TLB per coprire lo stesso ammontare di spazio degli indirizzi virtuali, riducendo i TLB miss. Questo comunque porterà all'aumento della frammentazione interna.<br>
5.  **D** - La presenza di una TLB garantisce sempre un tempo di accesso alla memoria più veloce rispetto a un sistema senza TLB? (SÌ/NO, Spiegare/motivare brevemente)<br>
    **R** - No. mentre una TLB generalmente aumenta i tempi di accesso alla memoria riducendo i lookups alla tabella delle pagine, in casi di un alto grado di TLB miss o di piccoli working sets che sono contenuti nella tabella delle pagine, i benefici potrebbero non essere significativi.
    Inoltre anche l'archittetura stessa del sistema influisce sulle prestazioni.<br>
6.  **D** - Se viene utilizzato l'algoritmo di sostituzione delle pagine LRU, garantisce il minor numero possibile di page fault? (SÌ/NO, Spiegare/motivare brevemente)<br>
    No. LRU è un'eccellente approssimazione pratica dell'algoritmo ottimo teorico, ma non garantisce il minimo assoluto di PF a causa dei limiti teorici (vs OPT) e della dipendenza dal pattern di accesso specifico del workload.
7.  **D** - Data la seguente stringa di riferimenti di memoria per un processo (indirizzamento a byte, con indirizzi espressi in codice esadecimale) e le rispettive operazioni di lettura(R)/scrittura(W):

    `R 1F40, W 3E20, R 5D18, W 7C9E, R 2AF3, W 6B2C, R 13B2, W 24AB, R 54BC, W 12AF`

    Assumere che il sistema abbia 3 frame disponibili e utilizzi l'algoritmo di sostituzione delle pagine Least Recently Used (LRU).
    * Calcolare la stringa dei riferimenti di pagina e indicare il numero di pagina corrispondente per ogni riferimento di memoria.
    * Indicare esplicitamente per ogni frame allocato e per ogni istante/riferimento il numero di pagina e se si verifica un page fault.
    * Calcolare il numero totale di page fault che si verificano durante questa sequenza di riferimenti di memoria.

**R** - per ottenere il numero di pagina, convertiamo l'indirizzo da esadecimale a decimale, quindi dividiamo per la dimensione della pagina (8192)<br>
Ad esempio <br>R 1F40 (hex) = 8000 (dec) → 8000 ÷ 8192 = 0 → Pagina 0 <br>
W 3E20 (hex) = 15904 (dec) → 15904 ÷ 8192 = 1 → Pagina 1<br>
In alternativa, dovremmo portare in binario l'indirizzo e fare lo shift di 13 bit (perché 8Kb = 2^13) <br>
ad esempio <br>
1F40 (hex) = 0001111101000000 (bin) <br>
Shift right di 13 bit → 000 = 0 (Pagina 0) <br>
<table>
  <thead>
    <tr>
      <th></th>
      <th>R 1F40</th>
      <th>W 3E20</th>
      <th>R 5D18</th>
      <th>W 7C9E</th>
      <th>R 2AF3</th>
      <th>W 6B2C</th>
      <th>R 13B2</th>
      <th>W 24AB</th>
      <th>R 54BC</th>
      <th>W 12AF</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><b>P #</b></td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <td><b>F 1</b></td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <td><b>F 2</b></td>
      <td></td>
      <td>0</td>
      <td>0</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <td><b>F 3</b></td>
      <td></td>
      <td></td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td><b>PF</b></td>
      <td>X</td>
      <td>X</td>
      <td>X</td>
      <td>X</td>
      <td></td>
      <td></td>
      <td>X</td>
      <td></td>
      <td>X</td>
      <td></td>
    </tr>
  </tbody>
</table>
<p>Total Page Fault = 6</p> 