### Domanda 1

Considera il seguente frammento di programma:

```c
#define N 32

data_t M[2*N][N];

int i,j,k;

for (k=0; k<4; k++) {
    for (i=0; i<2*N; i++) {
        for (j=k; j<N; j+=4) {
            M[i][j].diag = i-j;
        }
    }
}
```

Il codice macchina generato dal programma viene eseguito su un sistema con gestione della memoria basata su *demand paging*,
pagine di 2KB, politica di sostituzione delle pagine basata su *Working Set* (versione esatta), con delta=10.
Assumiamo che:

* `sizeof(data_t) == 1024` sia true.
* `data_t` abbia un campo `diag` di tipo `int` (dimensione 32 bit).
* Il segmento di codice (istruzioni macchina) abbia dimensione inferiore a una pagina.
* `M` sia allocata a partire dall'indirizzo logico `0x5820C800`.
* La matrice `M` sia allocata seguendo la strategia "row major" (prima la prima riga, seguita dalla seconda riga, ...).

Rispondi alle seguenti domande:

A) Quante pagine (e frame) sono necessarie per memorizzare la matrice?

B) Supponendo che le variabili i, j, e k siano allocate in registri (l'accesso a esse non produce riferimenti alla memoria),
quanti riferimenti $N_T = N_W + N_R$($N_R$ per la lettura e $N_W$ per la scrittura dei dati)  produce il programma proposto (non considerare i fetch delle istruzioni)?

C) Calcola il numero di *page fault* generati dal programma proposto (motiva/giustifica la risposta).

D) Completando opportunamente il seguente pezzo di programma, è possibile fornire un codice equivalente
(in termini di comportamento) al precedente, producendo meno *page fault*.
Completa il programma e calcola quanti *page fault* vengono generati (motiva la risposta).

```c
#define N 32

data_t M[2*N][N], *V;
int k;

V = &(M[0][0]);

for (k=0; k<2*N*N; k++) {
    V[k].diag = k/n - K%N;
}
```

---
**Risposte**

A) Il numero di elementi per pagina è dato da:<br>
N_per_pag = Dim_pagina / sizeof(data_t) = 2 KB / 1024 B = 2048 / 1024 = 2 elementi

Loop esterno k varia da 0 a 3
Loop medio i varia da i a 2*N, dove N=32 => i da 0 64;
Loop interno j varia da 0 a 32 escluso, ma a multipli di 4, 32/4 = 8, ma meno l'ultimo che non viene eseguito => 7

m[0][0] = 0; m[0][4] = -4; m[0][8] = -8 ... m[0][28] = -28<br>
m[1][0] = 1; m[1][4] = -3; m[1][8] = -7 ... m[1][28] = -27<br>
...<br>
m[63][0] = 63; m[63][4] = 59; m[63][8] = 55 .. m[63][28] = 35<br>
ma poi k cambia, quindi avremo ulteriori cicli dove andrà a scrivere gli elementi in precedenza saltati<br>
ad esempio per k=1, si avrà j=1, poi 5 ecc che erano stati saltati, andando a popolare a tutti gli elementi.<br>
Quindi possiamo dire che in generale questo programma popola la matrice M in maniera non ottimale<br>

N_elementi_tot = righe*colonne = 64*32 = 2048 elementi;<br>
N_pagine_tot = N_elem/capienza_pag = 1024 pagine(frame);

B) Abbiamo appurato che tutta la matrice viene vista e visitata una volta.<br>
N<sub>R</sub> = 0; => questo perché la lettura degli indici non produce riferimenti alla memoria<br>
N<sub>W</sub> = 2048;<br>
N<sub>T</sub>=N<sub>W</sub>+N<sub>R</sub>= 0 + 2048 = 2048

C) Dato che la politica di accesso è row-major, cioè è allocata per righe, per com'è definità attualmente la matrice: <br>
Abbiamo un working set che può contenere 10 Frame; sapendo che j salta di 4 indici per volta, i primi 10 accessi saranno sicuramente PF:<br>
Saltando 4 elementi, gli accessi saranno distanziati da 2 pagine (dato che ogni pagina può contenere 2 elementi).<br>
Abbiamo detto che le iterazioni di j sono 8, pertanto avremo sicuramente i primi 8 PF iniziali, ma cambiando riga all'iterazione <br>
di i successiva avremo cambiato nuovamente pagina, dato che salviamo per row-major (pagina [0][0] e [1][0] stanno in pagine diverse)

pertanto potremmo dedurre che i pf saranno dati da j*i*k = 8*4*64 = 2048 PF
Il motivo è che il delta=10 del Working Set è estremamente piccolo rispetto alla quantità di dati e al pattern di accesso della matrice.

Per una singola riga i e un k fisso, accediamo a 8 pagine. Queste 8 pagine rimangono nel WS.
Ma quando i cambia, si passa a una nuova riga. Ogni riga ha 32/2 = 16 pagine. Il salto da una riga all'altra è di 16 pagine.
Ogni "passata" di i (64 righe) con un k fisso causa 64 * 8 = 512 PF.
Quando k cambia (da 0 a 1, per esempio), il set di colonne a cui si accede è diverso. Anche se alcune pagine potrebbero
essere state condivise a livello di coppia (es. M[i][0] e M[i][1] nella stessa pagina), il numero enorme di accessi
(2048 totali) e il WS di soli 10 fanno sì che le pagine vengano continuamente rimosse e ricaricate.

In pratica, data l'estrema sparsità degli accessi nel loop j (salto di 4 elementi, che sono 2 pagine) e la transizione
ad una nuova riga (i) che implica un salto di ben 16 pagine, il Working Set di 10 riferimenti è troppo piccolo per
mantenere in memoria le pagine necessarie. Ogni nuovo accesso, in questo contesto, causerà molto probabilmente un Page Fault.

D) La soluzione linearizza la matrice in un vettore, trasformando gli accessi sparsi (j+=4) in accessi sequenziali
(k++). Questo massimizza la località spaziale (ogni elemento è adiacente al precedente), riducendo i page fault da 2048 a solo 2.
Avremo infatti un PF per la prima pagina (elementi da 0 a 1023) e un PF per la seconda (elementi da 1024 a 2047).
