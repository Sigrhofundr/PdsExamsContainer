# Esame 08 Luglio 2022 - OS Internals

## Domanda 1

Si consideri, per un dato processo, la seguente stringa di accessi alla memoria.
Per ogni indirizzo (indirizzamento Byte, con indirizzi espressi in codice esadecimale) viene riportata anche l'operazione di lettura (R) / scrittura (W):
`R 33FB, R 2264, W 0AD3, W 237E, R 08C8, W 13D7, R 094A, R 3465, W 32A0, R 1BB0, W 0CE5, R 1480, R 3294, R 2AB8`.

Supponiamo che gli indirizzi fisici e logici siano rappresentati su 16 bit, la dimensione della pagina sia 2 KByte e 5D02 sia l'indirizzo massimo utilizzabile dal programma (il limite superiore dello spazio degli indirizzi).

A. Calcolare la dimensione dello spazio di indirizzamento (espressa come numero di pagine) e la frammentazione interna.

B. Calcolare la stringa degli accessi alle pagine.

C. Simulare un algoritmo di sostituzione delle pagine di tipo PFF (Page Fault Frequency), assumendo un'unica soglia di tempo C = 2.<br>
Rappresentare il resident set (frame fisici contenenti pagine logiche) dopo ogni accesso alla memoria.
Indicare esplicitamente le attivazioni dell'algoritmo (la procedura di selezione della vittima). <br>
Rappresentare anche (con un pedice o altra notazione) i bit di riferimento associati a pagine / frame.

L'algoritmo PFF (approssimato) può essere riassunto come segue:
```
L'algoritmo viene attivato ad ogni page fault, non ad ogni accesso a pagina.
A seconda dell'intervallo di tempo TAU dal page fault precedente:
    se TAU < C, cioè se la frequenza di page fault è maggiore di quella desiderata, aggiungere un nuovo frame al Resident Set del processo che ha fatto page faulting.
    se TAU  >= C, cioè se la frequenza di page fault è OK, rimuovere dal resident set del processo che ha fatto page fault tutte le pagine con bit di riferimento a 0; quindi azzerare (impostare 0) il bit di riferimento di tutte le altre pagine nel Resident Set.

NOTA: quando si valuta TAU, considerare solo gli istanti di tempo "tra" i due page fault, quindi ad esempio quando si verificano due page fault ai tempi 7 e 8, TAU = 0, quando due page fault sono ai tempi 12 e 15, TAU = 2. I bit di riferimento vengono posti a 1 a ciascun accesso e azzerati come ultimo passaggio dell'algoritmo di sostituzione. Quindi, ogni volta che i bit di riferimento vengono azzerati, questo include la pagina relativa al page fault (il riferimento corrente).
```
 <table>
    <thead>
      <tr>
        <th >Tempo</th>
        <th>0</th>
        <th>1</th>
        <th>2</th>
        <th>3</th>
        <th>4</th>
        <th>5</th>
        <th>6</th>
        <th>7</th>
        <th>8</th>
        <th>9</th>
        <th>10</th>
        <th>11</th>
        <th>12</th>
        <th>13</th>
      </tr>
      <tr>
        <th >Accessi</th>
        <th>6</th>
        <th>4</th>
        <th>1</th>
        <th>4</th>
        <th>1</th>
        <th>2</th>
        <th>1</th>
        <th>6</th>
        <th>6</th>
        <th>3</th>
        <th>1</th>
        <th>2</th>
        <th>6</th>
        <th>5</th>
      </tr>
    </thead>
<tbody>
      <tr>
        <td>Resident Set</td> 
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
      </tr>
      <tr>
        <td></td> 
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
      </tr>
      <tr>
        <td ></td> 
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
      </tr>
      <tr>
        <td></td> 
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
      </tr>
        <tr>
        <td>Page Fault</td> 
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
      </tr>
<tr>
        <td>Attivazione<br>Algoritmo</td> 
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
      </tr>
    </tbody>
  </table>

D. Mostrare i page fault (accessi a pagine esterne al resident set) e calcolare il loro conteggio complessivo.

I page fault sono stati mostrati nella tabella.

Numero totale di PF:

---

