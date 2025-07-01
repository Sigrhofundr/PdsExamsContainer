### Domanda 1

Si consideri, per un dato processo, la seguente stringa di accessi alla memoria. Per ogni indirizzo (indirizzamento per Byte,
con indirizzi espressi in codice esadecimale) viene riportata anche l'operazione di lettura (R) / scrittura (W):<br>
`R 63F5, R 5A64, W 1AD3, W 6E7E, R 18C8, W 23D1, R 194E, R 5465, W 62A0, R 3BBA, W 1CE6, R 1480, R 6294, R 5AB8`. <br>
Si supponga che gli indirizzi fisici e logici siano rappresentati su 16 bit, la dimensione della pagina sia 4 KByte e `A817`
sia l'indirizzo massimo utilizzabile dal programma (il limite superiore dello spazio degli indirizzi).

A) Calcolare la dimensione dello spazio di indirizzamento (espressa come numero di pagine) e la frammentazione interna.<br>
B) Simulare un algoritmo di sostituzione delle pagine di tipo LRU (Least Recently Used), supponendo di avere a disposizione 3 frame.
Rappresentare il resident set (frame fisici contenenti pagine logiche) dopo ogni accesso alla memoria. <br>
C) Mostrare i page fault (accessi a pagine esterne al resident set) e calcolare il loro conteggio complessivo.<br>
D) spiegare brevemente perché la versione esatta dell'algoritmo LRU è inefficiente e quindi necessita di versioni approssimate.

---

**Risposte**

Riepilogo:
- indirizzi 16 bit -> 2 B
- Dim_pagina = 4 KB = 4096 B
- Lim_max_ind = 0xA817 = 43031_dec

A) N_pagine = ceil((Ind_max +1) / dim_pag) = ceil(43032/4096) = ceil(10.50) = 11<br>
Frammentazione = Dim_pag - ((ind_max+1) %dim_gina) = 4096 - (43032 - 4096*floor(43032/4096)) = 2024 B

B-C) Per ottenere il corrispettivo numero di pagina, basta dividere l'indirizzo per la dim_pagina

<table>
  <thead>
    <tr>
      <th>Addr</th>
      <th>R 63F5</th>
      <th>R 5A64</th>
      <th>W 1AD3</th>
      <th>W 6E7E</th>
      <th>R 18C8</th>
      <th>W 23D1</th>
      <th>R 194E</th>
      <th>R 5465</th>
      <th>W 62A0</th>
      <th>R 3BBA</th>
      <th>W 1CE6</th>
      <th>R 1480</th>
      <th>R 6294</th>
      <th>R 5AB8</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><b>P #</b></td>
      <td>6</td>
      <td>5</td>
      <td>1</td>
      <td>6</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>5</td>
      <td>6</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>6</td>
      <td>5</td>
    </tr>
    <tr>
      <td><b>F 1</b></td>
      <td>6</td>
      <td>6</td>
      <td>6</td>
      <td>6</td>
      <td>6</td>
      <td>6</td>
      <td>6</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <td><b>F 2</b></td>
      <td></td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>6</td>
      <td>6</td>
      <td>6</td>
      <td>6</td>
      <td>6</td>
      <td>6</td>
    </tr>
    <tr>
      <td><b>F 3</b></td>
      <td></td>
      <td></td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>5</td>
    </tr>
    <tr>
      <td><b>PF</b></td>
      <td>*</td>
      <td>*</td>
      <td>*</td>
      <td></td>
      <td></td>
      <td>*</td>
      <td></td>
      <td>*</td>
      <td>*</td>
      <td>*</td>
      <td>*</td>
      <td></td>
      <td></td>
      <td>*</td>
    </tr>
  </tbody>
</table>
<p>Total Page Fault = 9</p> 

D) L'inefficienza è relativa alla necessita di un supporto hardware per la sua implementazione, dato che per essere implementato in due soluzioni: 
- con dei contatori aggiuntivi associati agli elementi nella tabella delle pagine in base all'utilizzo, che vanno aggiornati per ogni accesso alla memoria;
- stack. viene creato uno stack dei numeri delle pagine, dove l'elemento in cima è l'ultimo utilizzato e quello in fondo è quello più "antico". Andranno aggiornati 
i puntatori agli elementi in questo stack (nel caso peggiore tutti).

Pertanto, questo sovraccarico di aggiornamento potrebbe rallentare di molto la memoria e l'uso di un SO, e per molti sistemi sarebbe troppo oneroso.

**altra versione**
L’algoritmo LRU esatto è inefficiente perché, per sapere qual è la pagina meno recentemente usata, bisogna aggiornare continuamente (ad ogni accesso in memoria) 
informazioni aggiuntive per ogni pagina, come un contatore (timestamp) oppure gestire uno stack doppiamente linkato. Queste operazioni vanno fatte sempre, 
non solo in caso di page fault, e sono troppo onerose in termini di tempo e risorse hardware. Per questo motivo si preferiscono versioni approssimate di LRU, 
che riducono il costo degli aggiornamenti e sono più pratiche da implementare nei sistemi reali.