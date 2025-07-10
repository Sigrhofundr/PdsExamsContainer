## Domanda 1 (comprende le domande da 1 a 5 del questionario)

### Domanda 1

Si consideri il seguente frammento di programma:
```c
#define N 64
data_t M[N][N/2];
int i,j,k;
...
for (k=0; k<4; k++) {
    for (i=0; i<N; i++) {
        for (j=k; j<N/2; j+=4) {
            M[i][j].num = i*j;
        }
    }
}
```

Il codice macchina generato dal programma viene eseguito su un sistema con gestione della memoria basata sulla paginazione
a richiesta, pagine da 2 KB, sostituzione delle pagine guidata da una politica LRU.

Supponiamo che:<br>
* sizeof(data_t) == 512 è vero
* data_t ha un campo num di tipo int (dimensione 32 bit)
* resident set = 32
* il segmento di codice (istruzioni macchina) ha una dimensione inferiore a una pagina
* M viene assegnato a partire dall'indirizzo logico $0x3870A400$
* la matrice M viene allocata seguendo la strategia _row major_, cioè per righe (prima riga, seguita dalla seconda riga, …);

**Rispondere alle seguenti domande:** 512

Quante pagine (e frame) sono necessarie per memorizzare la matrice (si richiede il conteggio esatto,
considerando, se ce ne fossero, pagine parzialmente usate)?

**Risposta**:

Per ricavare il numero di pagine, ci dobbiamo ricavare il numero totale di elementi e la dim della matrice;
* Elementi totali = $N * N/2$ = 64 * 32 = 2048;
* Dim_totale = Elementi_tot * $sizeof(data_t)$ = 2048 * 512 = 1.048.576 B
* Pagine necessarie = ceil(Dim_totale/Dim_pagina) = ceil(1.048.576 / 2048) =  512

**RISPOSTA REPORT PROF CABODI:** 513

---

### Domanda 2

Supponiamo che le variabili i, j e k siano allocate in registri della CPU. Quanti riferimenti a memoria produce il
programma proposto (indifferentemente per lettura o scrittura, senza considerare i fetch delle istruzioni)?

**Risposta**: 2048 **RISPOSTA REPORT PROF CABODI (CONFERMATO)**

Dobbiamo contare effettivamente i riferimenti, che sarà dato da:

Rif_tot = Σ(k=0 a 3) * Σ(i=0 to N-1) * (numero di j validi per k) = Σ(k=0 to 3) × N × (iterazioni di j per k) = <br>
= N × Σ(k=0 to 3) (iterazioni di j per k) = 64 * 32 = 2048

---

### Domanda 3

Calcolare il numero di page fault generati dal programma proposto.

**RISPOSTA REPORT PROF CABODI:** 2048

<!-- Per calcolare il numero di page fault generati dal programma, bisogna innanzitutto determinare il numero di pagine di memoria occupate dalla matrice M.

La matrice M è composta da 64 righe e 32 colonne ($M[64][32]$). Ogni elemento di tipo data_t occupa 512 byte. Quindi, una riga occupa:<br>
32 * 512 byte = 16.384 byte<br>
Dato che la dimensione di una pagina è 2 KB, ogni riga occupa:<br>
16.384/2048 = 8 pagine<br>
Essendoci 64 righe, la matrice occupa in totale:
64 * 8 = 512 pagine

Il programma accede a tutti gli elementi della matrice, seguendo un pattern che non sfrutta la località spaziale 
(gli accessi saltano tra colonne distanti e passano rapidamente da una riga all’altra). 
Considerando che il resident set è di sole 32 pagine, molto inferiore alle 512 necessarie per contenere tutta la matrice, 
dopo il caricamento iniziale delle prime 32 pagine, ogni nuovo accesso a una pagina non più presente in memoria causerà un page fault, secondo la politica LRU (Least Recently Used).

Poiché ogni pagina viene acceduta una sola volta, e non viene riutilizzata prima che venga espulsa dalla memoria, 
il numero di page fault sarà pari al numero totale di pagine distinte referenziate, cioè:

512 -->

---

### Domanda 4

Completando correttamente la seguente porzione di programma, è possibile fornire un codice equivalente (in
termini di comportamento) al precedente, producendo meno page fault. Completare il programma e calcolare
quanti page fault vengono generati (motiva la risposta)
```c
#define N 64
data_t M[N][N/2], *V;
int k;
...
V = &(M[0][0]);
for (k=0; k<N*N/2; k++) {
    V[k].num = ........ /* to be completed */;
} 
```
calcolare il numero di page fault

**Risposta**: 512

**RISPOSTA REPORT PROF CABODI:** 513

Il codice originale accede alla matrice con un pattern "a colonne saltate" (k, k+4, k+8, ...), mentre il nuovo codice vuole accedere sequenzialmente usando un puntatore V.<br>
Nel codice originale, per ogni elemento M[i][j] scritto, dobbiamo trovare la corrispondente posizione sequenziale k nel nuovo codice.
Mappatura row-major: L'elemento M[i][j] si trova alla posizione i * (N/2) + j nell'array lineare.
Ma nel nostro caso, non vogliamo tutti gli elementi, solo quelli che il codice originale scriveva!

Avremo:
* Dimensione matrice: N × (N/2) × sizeof(data_t) = 64 × 32 × 512 = 1,048,576 bytes
* Dimensione pagina: 2KB = 2,048 bytes
* Elementi per pagina: 2,048 / 512 = 4 elementi
* Numero totale di pagine: 1,048,576 / 2,048 = 512 pagine
  
Tuttavia, considerando il resident set di 32 pagine:

* Le prime 32 pagine vengono caricate senza sostituzioni
* Dalla 33ª pagina in poi, ogni nuova pagina causa un page fault e la sostituzione della pagina LRU
* Page fault totali: 512 (uno per ogni pagina, dato che l'accesso è strettamente sequenziale)




---

### Domanda 5

Motivare la risposta precedente (numero di page fault) e completare la parte mancante del programma<br>
`V[k].num = (k / (N/2)) * (k % (N/2));`

Dove:
* `k / (N/2)` ci dà la riga i;
* `k % (N/2)` ci dà la colonna j;
* `(k / (N/2)) * (k % (N/2))` equivale a i*j;

**RISPOSTA REPORT PROF CABODI:**
- motivazione:
  l'algoritmo originale fa 4 iterazioni su tutte le pagine, dopo che sono sicuramente uscite dal resident set
  con l'algoritmo modificato, dopo che una pagina esce dal resident set non viene più utilizzata (non vi si fa più
  accesso).
  Per questo motivo il numero di page fault diminuisce di un fattore 4 e corrisponde al numero di pagine.
- `V[k].num = (k/(N/2))*(k%(N/2))` // o altra formula equivalente. Il primo fattore corrisponde al numero di riga,
  il secondo al numero di colonna
---