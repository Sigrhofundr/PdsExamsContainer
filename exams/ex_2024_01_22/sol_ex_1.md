## Domanda 1 - Gestione della memoria virtuale: Paging

Nel contesto della gestione della memoria virtuale, si consideri un sistema con uno schema di paging:

A) Definire cos'è un `page fault` , spiegare le condizioni in cui si verifica un page fault e le
conseguenze per il sistema operativo.<br>
B) Elencare i passaggi coinvolti nella gestione di un page fault, illustrando come il sistema operativo risponde a un
page fault e il meccanismo che impiega per risolvere questi errori.<br>
C) Si consideri un array bidimensionale A: `int A[100][100]`, dove `A[0][0]` si trova alla posizione 800 in un sistema di
memoria paginato e byte-indirizzabile, con pagine di dimensione 800 Byte. La dimensione di un intero è di 32 bit.
Il processo che manipola la matrice risiede nella pagina 0 (posizioni da 0 a 799).
Pertanto, ogni recupero di istruzione (instruction fetch) avverrà dalla pagina 0. Considerando tre frame di pagina,
quanti page fault vengono generati dal seguente ciclo di inizializzazione dell'array? Utilizzare la politica di
sostituzione LRU (Least Recently Used) e assumere che un frame di pagina contenga il codice e gli altri due siano inizialmente vuoti.
```c
1. 
for (int j=0; j <100, j++)
    for (int i = 0; i < 100; i ++){
        A[i][j] = 0;
    }
2. 
for (int i=0; i <100, i++)
    for (int j = 0; j < 100; j ++){
        A[i][j] = 0;
   }
```

**Risposte**

A. Un page fault è un errore che si verifica quando un processo tenta l'accesso a una pagina che non era stata caricata in memoria.
Questo accade perché nei calcolatori moderni, si è capito che non era ottimale caricare in memoria tutte le pagine relative
al programma desiderato, dato che in molti caso alcune parti vengono usate raramente. Ciò ha portato allo sviluppo della paginazione su
richiesta. L'hardware che si occupa di questa paginazione, noterà che il bit di validità della pagina corrispondente risulterà
invalido, ciò genererà un trap per il SO. A questo punto il SO dovrà provvedere al caricamento in memoria, individuando un frame
libero per il caricamento, quindi eseguendo l'operazione I/O necessaria. Il SO, durante questo procedimento, può decidere
di eseguire nel frattempo un altro processo, implementando così il multitasking e ottimizzando l'utilizzo della CPU
durante i tempi, solitamente più lunghi, delle operazioni I/O.<br>
B. La procedura solitamente è la seguente e lineare:
1. Si controlla nella tabella interna al processo se il riferimento fosse un accesso alla memoria valido o no;
2. Se il riferimento non era valido, si termina il processo. Se era un rif valido, ma la pagina non era ancora stata portata
   in memoria, se ne effettua il caricamento;
3. Si individua un frame libero, per esempio prelevandone uno dalla lista dei frame liberi;
4. Si programma un'operazione sui dischi per trasferire la pagina desiderata nel frame appena assegnato;
5. Quando la lettura dal disco è completa, si modificano la tabella interna e la tabella delle pagine per indicare che
   la pagina si trova attualmente in memoria;
6. Si riavvia l'istruzione interrotta dall'eccezione. A questo punto il processo può accedere alla pagina come se questa
   fosse stata sempre presente in memoria;

C. Riepilogo dati:
- Array bidimensionale A[100][100] di interi a 32 bit (4B);
- Posizione iniziale: A[0][0] all'indirizzo 800;
- Dim Pagina: 800 B;
- 3 frame totali: 1 per il codice (pag 0), 2 per i dati;
- Algoritmo LRU;

- Capacità pagina: Dim Pagina / dim interi = 800 / 4 = 200 interi;
- Elementi Array : righe*colonne = 10000 elementi
- Numero pagine necessarie = Elementi array / capacità pag = 10000 /200 = 50 pagine per l'intero array;

- Disposizione in memoria:
  In un array bidimensionale in C, gli elementi sono memorizzati row-major order (per righe):

A[0][0], A[0][1], ..., A[0][99], A[1][0], A[1][1], ...



Il primo ciclo indica un accesso per colonne, che causerà parecchi page fault dato che ogni accesso potrebbe richiedere
una pagina diversa; il secondo invece accede agli elementi in ordine sequenziale, sfruttando meglio la località spaziale,
causando quindi meno page fault. Avendo a disposizione solo 2 frame disponibili per i dati, il numero di page fault aumenterà.

Nella prima matrice:
- scorrendo per colonne, avremo:

- Mappatura pagine:

Pagina 1: A[0][0] ... A[0][99], A[1][0] ... A[1][99]  (righe 0-1)
Pagina 2: A[2][0] ... A[2][99], A[3][0] ... A[3][99]  (righe 2-3)
Pagina 3: A[4][0] ... A[4][99], A[5][0] ... A[5][99]  (righe 4-5)
...
Pagina 25: A[48][0] ... A[48][99], A[49][0] ... A[49][99] (righe 48-49)
Pagina 26: A[50][0] ... A[50][99], A[51][0] ... A[51][99] (righe 50-51)
...
Pagina 50: A[98][0] ... A[98][99], A[99][0] ... A[99][99] (righe 98-99)

Formula per Calcolare la Pagina
Per un elemento A[i][j]: pagina = (i / 2) + 1 (divisione intera)

- Analisi dei page fault:
    - per j = 0 (prima colonna):
    - A[0][0] → Pagina 1 (page fault - carica pagina 1)
      A[1][0] → Pagina 1 (già in memoria)
      A[2][0] → Pagina 2 (page fault - carica pagina 2)
      A[3][0] → Pagina 2 (già in memoria)
      A[4][0] → Pagina 3 (page fault - carica pagina 3, sostituisce pagina 1 con LRU)
      A[5][0] → Pagina 3 (già in memoria)
      A[6][0] → Pagina 4 (page fault - carica pagina 4, sostituisce pagina 2 con LRU)
      ...
- da ciò emerge che:
- ogni 2 accessi consecutivi, serve una nuova pagina;
- con solo 2 frame disponibili, si ha un page fault ogni 2 accessi dopo i primi 4;
- i primi 4 accessi generano 3 page fault per riempire i frame;
- dopo, ogni nuovo accesso a una pagina non presente causa sostituzione;

Quindi, per ogni colonna j:
- si accede a tutte le 100 righe (50 pagine diverse);
- Pattern: 2 accessi senza PF, poi 1 PF, 1 accesso senza PF, poi 1 PF...

50 pagine × 100 colonne = 5000 "visite"
Con solo 2 frame e 50 pagine da visitare per colonna, ogni visita genera sostanzialmente un page fault
PF per matrice 1 = 50*100 = 5000;

Nella seconda matrice:

- l'accesso è sequenziale per righe. Ogni pagina contiene 2 righe complete.
  Quando processiamo una coppia di righe, generiamo 1 page fault per caricare la pagina, poi tutti gli accessi successivi
  nella stessa pagina sono hit. Con 50 coppie di righe, otteniamo 50 page fault totali.
