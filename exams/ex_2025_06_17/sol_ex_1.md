## Domanda 1 (comprende le domande da 1 a 4 del questionario)

Un sistema ha le seguenti caratteristiche:

* Dimensione della memoria fisica: 8 GB
* TLB con 128 entry
* Spazio di indirizzamento virtuale: indirizzi a 64 bit
* Dimensione pagina: 16 KB
* Utilizza una tabella di pagine gerarchica a tre livelli
* Implementa un algoritmo di sostituzione delle pagine Second Chance.

----

### Domanda 1

**Punteggio max.: 0,50**

A) Calcolare il TLB hit minimo necessario per garantire una riduzione massima del 40% del tempo di accesso effettivo (EAT), rispetto a un tempo RAM di 40 ns.
Esprimere il risultato come frazione di 1, con due cifre decimali (es. 0.85 o 0,85).
Si accetta un errore di 0.01.

**Risposta:**

---

### Domanda 2

**Punteggio max.: 0,50**

In che modo un allocatore Buddy affronta il problema della frammentazione?

* La frammentazione esterna viene eliminata allocando blocchi di dimensioni multiple di una pagina
* Con Buddy non è possibile alcuna frammentazione interna
* I blocchi liberi possono avere solo dimensioni pari a una potenza di 2, il che riduce il problema della frammentazione esterna
* La frammentazione esterna viene eliminata (è possibile solo la frammentazione interna)

-------------
### Domanda 3

**Punteggio max.: 0,50**

Una free list (di frame liberi) potrebbe essere ordinata, per supportare l'allocazione di frame contigui a un costo lineare.

* Per indirizzi sia fisici che logici **NO**
* Per dimensione decrescente **NO**
* Sorting has no impact on performance **YES**
* L'ordinamento non è necessario **YES**
* Solo per indirizzi fisici **NO**

-------------

### Domanda 4

**Punteggio max.: 1,50**

Data la seguente stringa di riferimento alla memoria  per un processo (indirizzamento in byte, con indirizzi espressi in codice esadecimale, omessi gli zeri iniziali) e le rispettive operazioni di lettura (R)/scrittura (W):

`R 1F040, W 3E320, R 5DB18, W 6C6E9, R 2A8F3, W 6D98C, R 13AB2, W 240AB, R 94B0C, W 12AF1`

Si supponga che il sistema abbia 3 frame disponibili e utilizzi un algoritmo di sostituzione di pagina Second Chance.
Si calcoli la stringa di riferimenti a pagina e si indichi il numero di pagina corrispondente per ciascun riferimento in memoria (indicare i numeri di pagina in esadecimale, senza Ox iniziale).
Si indichi esplicitamente, per ogni frame e tempo/riferimento, il numero di pagina, il reference bit (ad esempio, la pagina 2 con reference bit 1 può essere rappresentata con $2(1)$ o 2\_1; i reference bit delle pagine che causano page fault siano inizializzati a 0), e se si verifica un errore di pagina (page fault).
Si indichi anche, con un asterisco o altro simbolo, il frame/pagina in testa alla coda FIFO.
Indicare anche il numero totale di page faults.

| 1f040 | 3E320 | 1DB18 | 6C6E9 | 2A8F3 | 6D98C | 13AB2 | 240AB | 94B0C | 12AF1 |
|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|
| P#    |       |       |       |       |       |       |       |       |       | 
| F1    |       |       |       |       |       |       |       |       |       | 
| F2    |       |       |       |       |       |       |       |       |       | 
| F3    |       |       |       |       |       |       |       |       |       | 
| PF    |       |       |       |       |       |       |       |       |       |

Total Page Faults = 

---