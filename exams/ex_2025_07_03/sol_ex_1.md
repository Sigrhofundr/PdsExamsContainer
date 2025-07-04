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

**Rispondere alle seguenti domande:**

Quante pagine (e frame) sono necessarie per memorizzare la matrice (si richiede il conteggio esatto,
considerando, se ce ne fossero, pagine parzialmente usate)?

**Risposta**:

---

### Domanda 2

Supponiamo che le variabili i, j e k siano allocate in registri della CPU. Quanti riferimenti a memoria produce il
programma proposto (indifferentemente per lettura o scrittura, senza considerare i fetch delle istruzioni)?

**Risposta**:

---

### Domanda 3

Calcolare il numero di page fault generati dal programma proposto.

**Risposta**:

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

**Risposta**:

---

### Domanda 5

Motivare la risposta precedente (numero di page fault) e completare la parte mancante del programma
`V[k].num = ...`

---