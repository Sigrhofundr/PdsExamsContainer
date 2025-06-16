## Domanda 1: Gestione della Memoria e Algoritmo di Sostituzione delle Pagine

Considera la seguente stringa di riferimenti di memoria, per un dato processo. Per ogni riferimento (indirizzamento a Byte, con indirizzi espressi in codice esadecimale)
è riportata anche l'operazione di lettura(R)/scrittura(W): R 33FB, R 1B64, W 30D3, W 237E, R 0AC8, W 23D7, R 174A, R 0965, W 32A0, R 1BB0, W 09E5, R 3380, R 2A94, R 11B8.
Assumi che gli indirizzi fisici e logici siano a 16 bit, la dimensione della pagina sia 2 KByte e 70FF sia l'indirizzo massimo utilizzabile dal programma (il limite superiore dello spazio di indirizzamento).<br>
A) **D** - Calcola la dimensione dello spazio di indirizzamento (espressa come numero di pagine) e la frammentazione interna.<br>
**R** - Riepilogando: <br>
Indirizzi = 16 bit => 2 B; Dim_Pagina = 2KB = 2 * 1024 B = 2048 B = 2^11; Ind Max Programma = 70FF_ex = 28927_dec < 2^15 <br>
DIM_SPAZIO = IND_MAX + 1 => 70FF +1 = 7100 => 28928 B = 28KB + 256B
#_pagine = arr_per_eccesso(Dimensione_spazio/Dim_Pagina) = arr_ex(28928/2048) = 14,125 => 15
Spazio utilizzato = DIM_SPAZIO
Spazio allocato = Pagine_necessarie * DIM_PAG = 15 * 2048= 30720
Frammentazione interna = Spazio allocato - utilizzato = 30720 - 28928 = 1792 B
B) **D** - Calcola la stringa dei riferimenti di pagina.<br>
**R** - Intanto:<br>
per ottenere il numero di pagina, convertiamo l'indirizzo da esadecimale a decimale, quindi dividiamo per la dimensione della pagina (2048) (si potrebbe usare anche il metodo dello shift):<br>
6 3 6 4 1 4 2 1 6 3 1 6 5 2<br>
C) **D** - Simula un algoritmo di sostituzione delle pagine Second-Chance, con 3 frame disponibili. Rappresenta il set residente (frame fisici contenenti pagine logiche) dopo ogni riferimento
di memoria. Indica esplicitamente, per ogni frame allocato, il numero di pagina e il bit di riferimento (usa la notazione p,r o pr).
Indica anche i page fault. Assumi che il bit di riferimento di una pagina sia inizializzato a 0 dopo un page fault.<br>
<table>
  <thead>
    <tr>
      <th style="background-color: #f2f2f2;">Time</th>
      <th style="background-color: #f2f2f2;">0</th>
      <th style="background-color: #f2f2f2;">1</th>
      <th style="background-color: #f2f2f2;">2</th>
      <th style="background-color: #f2f2f2;">3</th>
      <th style="background-color: #f2f2f2;">4</th>
      <th style="background-color: #f2f2f2;">5</th>
      <th style="background-color: #f2f2f2;">6</th>
      <th style="background-color: #f2f2f2;">7</th>
      <th style="background-color: #f2f2f2;">8</th>
      <th style="background-color: #f2f2f2;">9</th>
      <th style="background-color: #f2f2f2;">10</th>
      <th style="background-color: #f2f2f2;">11</th>
      <th style="background-color: #f2f2f2;">12</th>
      <th style="background-color: #f2f2f2;">13</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>References</td>
      <td>6</td>
      <td>3</td>
      <td>6</td>
      <td>4</td>
      <td>1</td>
      <td>4</td>
      <td>2</td>
      <td>1</td>
      <td>6</td>
      <td>3</td>
      <td>1</td>
      <td>6</td>
      <td>5</td>
      <td>2</td>
    </tr>
    <tr>
      <td>Resident Set</td>
      <td>6₀</td>
      <td>6₀</td>
      <td>6₁</td>
      <td style="background-color: #DDDDDD;">6₁</td>
      <td>6₀</td>
      <td>6₀</td>
      <td>2₀</td>
      <td>2₀</td>
      <td style="background-color: #DDDDDD;">2₀</td>
      <td>3₀</td>
      <td>3₀</td>
      <td>3₀</td>
      <td>5₀</td>
      <td>5₀</td>
    </tr>
    <tr>
      <td></td>
      <td>3₀</td>
      <td>3₀</td>
      <td>3₀</td>
      <td>3₀</td>
      <td>1₀</td>
      <td>1₀</td>
      <td style="background-color: #DDDDDD;">1₀</td>
      <td style="background-color: #DDDDDD;">1₁</td>
      <td>1₀</td>
      <td style="background-color: #DDDDDD;">1₀</td>
      <td style="background-color: #DDDDDD;">1₁</td>
      <td style="background-color: #DDDDDD;">1₁</td>
      <td style="background-color: #DDDDDD;">1₀</td>
      <td>2₀</td>
    </tr>
    <tr>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td>4₀</td>
      <td style="background-color: #DDDDDD;">4₀</td>
      <td style="background-color: #DDDDDD;">4₁</td>
      <td>4₀</td>
      <td>4₀</td>
      <td>6₀</td>
      <td>6₀</td>
      <td>6₀</td>
      <td>6₁</td>
      <td>6₀</td>
      <td style="background-color: #DDDDDD;">6₀</td>
    </tr>
    <tr>
      <td>Page Faults</td>
      <td>*</td>
      <td>*</td>
      <td></td>
      <td>*</td>
      <td>*</td>
      <td></td>
      <td>*</td>
      <td></td>
      <td>*</td>
      <td>*</td>
      <td></td>
      <td></td>
      <td>*</td>
      <td>*</td>
    </tr>
  </tbody>
</table>