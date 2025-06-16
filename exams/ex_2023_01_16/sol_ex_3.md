## Domanda 3

Si consideri un sistema con paginazione. Si dica, per ognuna delle caratteristiche elencate,
se sia preferibile una dimensione di pagina maggiore o minore, motivando la risposta.

<table border="1">
  <thead>
    <tr>
      <th></th>
      <th>PT più grande (spiega perché)</th>
      <th>PT più piccolo (spiegare perché)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Frammentazione</td>
      <td></td>
      <td>Pagine più piccole riducono la frammentazione interna (max = dimensione pagina), ma aumentano l'overhead di gestione.</td>
    </tr>
    <tr>
      <td>Dimensione PT</td>
      <td>Pagine più grandi riducono il numero di entry nella page table, diminuendo la memoria necessaria per la PT stessa.</td>
      <td></td>
    </tr>
    <tr>
      <td>Overhead I/O</td>
      <td>Pagine più grandi permettono trasferimenti più efficienti (meno operazioni I/O per la stessa quantità di dati).</td>
      <td></td>
    </tr>
    <tr>
      <td>Numero di page fault</td>
      <td>Pagine più grandi riducono i page fault grazie a maggiore località spaziale.</td>
      <td></td>
    </tr>
    <tr>
      <td>Località</td>
      <td>Pagine grandi sfruttano meglio la località spaziale ma possono caricare dati inutili. </td>
      <td>Pagine piccole sono più precise ma richiedono più page fault.</td>
    </tr>
    <tr>
      <td>Dimensioni/copertura TLB</td>
      <td>Pagine più grandi coprono più memoria con meno entry TLB, migliorando il hit rate.</td>
      <td></td>
    </tr>
  </tbody>
</table>