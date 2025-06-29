### Domanda 3

A) Un file di testo di grandi dimensioni contiene N righe di lunghezza fissa (50 caratteri per ogni riga). Dobbiamo ordinare le righe nel file usando un algoritmo di ordinamento con complessità linearitmica ($O(NlogN)$).
A causa delle dimensioni, l'ordinamento viene implementato direttamente sul file (sfruttando la mappatura in memoria dei file o utilizzando opportunamente le primitive `seek`, `read` e `write`).<br>
A seconda della strategia di allocazione del file system sottostante, quale complessità complessiva ci aspettiamo per il compito di ordinamento? (per ogni strategia di allocazione del file, scegli la complessità e fornisci una breve motivazione)

<table>
<thead>
<tr>
<th></th>
<th>O(N)</th>
<th>O(NlogN)</th>
<th>O(N²)</th>
<th>O(N²logN)</th>
<th>O(N³)</th>
<th>Motivazione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Contiguous</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>Linked list</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>FAT</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>Inodes</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

B) Un grande insieme di dati è memorizzato su file utilizzando la strategia "indice e file relativo": il file indice contiene una sequenza di N record di dimensione fissa,
ciascuno contenente una chiave di ricerca e un puntatore (un Indirizzo Logico) al record corrispondente nel file relativo.
Il file indice è ordinato per chiavi crescenti. Il file relativo contiene N record di lunghezza variabile. I record NON sono ordinati.<br>
A seconda della strategia di allocazione del file system sottostante, quale complessità complessiva ci aspettiamo per
l'operazione di stampa di tutti i dati in ordine crescente di chiavi? (per ogni strategia di allocazione del file,
scegli la complessità e fornisci una breve motivazione)

<table>
<thead>
<tr>
<th></th>
<th>O(logN)</th>
<th>O(N)</th>
<th>O(NlogN)</th>
<th>O(N²)</th>
<th>Motivazione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Contiguous</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>Linked list</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>FAT</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
<tr>
<td>Inodes</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

---
**Risposte**

A) Commento generale: La complessità dell'algoritmo va moltiplicata per il costo di accesso ai dati (diretto/randomico) a un dato indirizzo logico all'interno del file.


<table>
<thead>
<tr>
<th></th>
<th>O(N)</th>
<th>O(NlogN)</th>
<th>O(N²)</th>
<th>O(N²logN)</th>
<th>O(N³)</th>
<th>Motivazione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Contiguous</td>
<td></td>
<td>X</td>
<td></td>
<td></td>
<td></td>
<td>L'accesso diretto è supportato (O(1)). Non avremo overhead di ricerca.<br>Conversione LA->PA istantanea; Predizione branch efficace</td>
</tr>
<tr>
<td>Linked list</td>
<td></td>
<td></td>
<td></td>
<td>X</td>
<td></td>
<td>Non è possibile accesso diretto, l'accesso è lineare (O(N)). Pessima per accesso casuale<br>Cache miss frequenti per accessi sparsi. Degrada drasticamente con file grandi</td>
</tr>
<tr>
<td>FAT</td>
<td></td>
<td></td>
<td></td>
<td>X</td>
<td></td>
<td>Lettura sequenziale con lookup FAT (O(N)), Overhead minimo per accesso sequenziale<br>Migliore di linked list per accesso casuale, peggiore di contiguous per overhead di dereferenziazione</td>
</tr>
<tr>
<td>Inodes</td>
<td></td>
<td>X</td>
<td></td>
<td></td>
<td></td>
<td>Accesso diretto (O(1)), ottimizzazione tramite caching degli indici<br>Struttura ad albero bilanciata.Località negli accessi agli indici</td>
</tr>
</tbody>
</table>

B) Commento generale: la stampa può servire l'ordine di file degli indici, quindi è lineare O(N). Per ogni record, il corrispondente
indirizzo logico è ottenuto a tempo costante. Quindi, la complessità dell'algoritmo deve essere moltiplicata per il costo di un
diretto/casuale accesso al dato indirizzo logico all'interno del file.

<table>
<thead>
<tr>
<th></th>
<th>O(logN)</th>
<th>O(N)</th>
<th>O(NlogN)</th>
<th>O(N²)</th>
<th>Motivazione</th>
</tr>
</thead>
<tbody>
<tr>
<td>Contiguous</td>
<td></td>
<td>X</td>
<td></td>
<td></td>
<td>Ottimizzazioni possibili: prefetching sequenziale; buffer di lettura ottimizzato;<br>sfruttamento cache del SO. Accesso diretto O(1)</td>
</tr>
<tr>
<td>Linked list</td>
<td></td>
<td></td>
<td></td>
<td>X</td>
<td>Ogni accesso indirizzo logico richiede l'attraversamento dall'inizio.<br> è O(N^<sup>2</sup>) = N accessi x O(N) attravversamento<br>
non è migliorabile senza ristrutturazione. Quindi non ha accesso diretto</td>
</tr>
<tr>
<td>FAT</td>
<td></td>
<td></td>
<td></td>
<td>X</td>
<td>Miglioramenti: caching della FAT in memoria; prefetching entry FAT correlati;<br> Batch reading per ridurre syscall</td>
</tr>
<tr>
<td>Inodes</td>
<td></td>
<td>X</td>
<td></td>
<td></td>
<td>Accesso diretto supportato O(1). Ottimizzazioni: extended-base allocation per ridurre frammentazione;<br> delayed allocation per ottimizzare layout; read-ahead intelligente basato su pattern</td>
</tr>
</tbody>
</table>
