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

**Risposta:** $TLB_{Hit Minimo}$ =


considerato che EAT = (TLB_Hit_Rate × Tempo_TLB_Hit) + (TLB_Miss_Rate × Tempo_TLB_Miss) <br>
TLB_Miss_Rate = 1 - TLB_Hit_Rate<br>
Tempo_TLB_Hit = Tempo_accesso_TLB + Tempo_accesso_RAM <br>
Tempo_TLB_Miss = Tempo_accesso_TLB + (Livelli_PT × Tempo_accesso_RAM) + Tempo_accesso_RAM <br>

EAT = hit_rate_TLB * T_acc_RAM + (1-hit_rate_TLB) * 4 * T_acc_RAM

considerato che vogliamo che  EAT ≤ 1.4 × T_acc_RAM, sostituiamo

hit_rate_TLB * T_acc_RAM + (1-hit_rate_TLB) * 4 * T_acc_RAM <= 1.4 * T_acc_RAM<br>
40 ns hit_rate_TLB + (1-hit_rate_TLB) 160ns <= 1.4 * 40 ns<br>
40 ns * hit_rate_TLB + 160 ns - 160ns hit_rate_tlb <= 1.4 * 40 ns<br>
-120 ns hit_rate_TLB + 160 ns <= 56 ns<br>
160 ns - 56 ns <= 120 ns * hit_rate_tlb<br>
104 ns <= 120 ns * hit_rate_tlb<br>
104/120 ns <= hit_rate_tlb 

hit_rate_tlb >= 0.8666666667 circa 0.87 

---

### Domanda 2

**Punteggio max.: 0,50**

In che modo un allocatore Buddy affronta il problema della frammentazione?

* La frammentazione esterna viene eliminata allocando blocchi di dimensioni multiple di una pagina **No**

**Spiegazione**

Questa affermazione confonde il sistema Buddy con altri sistemi di allocazione. Nel Buddy system, i blocchi hanno dimensioni che sono potenze di 2, non necessariamente multipli di una pagina. 
Inoltre, il Buddy system può ancora soffrire di frammentazione esterna quando blocchi liberi adiacenti non possono essere coalescati perché non sono "buddy" (non formano una coppia valida secondo le regole del sistema).

* Con Buddy non è possibile alcuna frammentazione interna **No**

**Spiegazione**

Il Buddy system soffre significativamente di frammentazione interna. Quando si richiede memoria di dimensione non potenza di 2, il sistema alloca il blocco più piccolo che sia potenza di 2 e maggiore o uguale alla richiesta. Ad esempio, per 65KB si allocano 128KB, sprecando 63KB (quasi il 50%). Questa è proprio una delle principali debolezze del Buddy system.

* I blocchi liberi possono avere solo dimensioni pari a una potenza di 2, il che riduce il problema della frammentazione esterna **Sì**

**Spiegazione**

Questa è corretta. Il vincolo delle potenze di 2 semplifica la gestione della memoria e facilita la coalescenza dei blocchi. 
Quando due buddy adiacenti vengono liberati, possono essere immediatamente fusi in un blocco di dimensione doppia. Questo meccanismo di coalescenza predicibile ed efficiente aiuta a ridurre (ma non elimina) la frammentazione esterna.

* La frammentazione esterna viene eliminata (è possibile solo la frammentazione interna) **No**

**Spiegazione**

Il Buddy system può soffrire di entrambi i tipi di frammentazione. La frammentazione esterna si verifica quando ci 
sono blocchi liberi sufficienti in totale, ma non c'è un singolo blocco contiguo abbastanza grande. 
Esempio: se ho due blocchi liberi da 64KB non adiacenti (o non buddy), non posso soddisfare una richiesta di 128KB anche se ho 128KB liberi totali.

-------------
### Domanda 3

**Punteggio max.: 0,50**

Una free list (di frame liberi) potrebbe essere ordinata, per supportare l'allocazione di frame contigui a un costo lineare.

* Per indirizzi sia fisici che logici **NO**

**Spiegazione**

Una free list di frame liberi contiene solo indirizzi fisici. I frame sono unità di memoria fisica, non logica. Gli indirizzi logici appartengono allo spazio di indirizzamento virtuale dei processi e sono mappati ai frame fisici tramite la page table. Non ha senso ordinare frame fisici per indirizzi logici che non esistono ancora o che dipendono dal processo.

* Per dimensione decrescente **NO**

**Spiegazione**

I frame hanno tutti la stessa dimensione fissa (tipicamente 4KB). Non ha senso ordinare per dimensione elementi che sono tutti uguali. Questa affermazione confonde i frame con segmenti o blocchi di dimensione variabile. Nel contesto della gestione dei frame, tutti hanno dimensione identica per definizione.

* L'ordinamento non è necessario **NO**

**Spiegazione**

Se vogliamo supportare l'allocazione di frame contigui a costo lineare (come specificato nel problema), l'ordinamento È necessario. Senza ordinamento, trovare N frame contigui richiede algoritmi più complessi e costosi. L'ordinamento per indirizzo fisico permette di identificare facilmente sequenze contigue con una semplice scansione.


* Solo per indirizzi fisici **Sì**

**Spiegazione**

Questa è la risposta corretta. Per poter identificare e allocare efficientemente blocchi di memoria fisicamente contigui, la free list deve essere ordinata in base all'indirizzo fisico di inizio dei frame. Questo permette di scorrere la lista e verificare facilmente se un frame libero è adiacente al successivo, consentendo la fusione (coalescenza) di blocchi più piccoli in uno più grande e la rapida individuazione di segmenti contigui di memoria.


-------------

### Domanda 4

**Punteggio max.: 1,50**

Data la seguente stringa di riferimento alla memoria per un processo (indirizzamento in byte, con indirizzi espressi in codice esadecimale, omessi gli zeri iniziali) e le rispettive operazioni di lettura (R)/scrittura (W):

`R 1F040, W 3E320, R 5DB18, W 6C6E9, R 2A8F3, W 6D98C, R 13AB2, W 240AB, R 94B0C, W 12AF1`

Si supponga che il sistema abbia 3 frame disponibili e utilizzi un algoritmo di sostituzione di pagina Second Chance.
Si calcoli la stringa di riferimenti a pagina e si indichi il numero di pagina corrispondente per ciascun riferimento in memoria (indicare i numeri di pagina in esadecimale, senza Ox iniziale).
Si indichi esplicitamente, per ogni frame e tempo/riferimento, il numero di pagina, il reference bit (ad esempio, la pagina 2 con reference bit 1 può essere rappresentata con $2(1)$ o 2\_1; i reference bit delle pagine che causano page fault siano inizializzati a 0), e se si verifica un errore di pagina (page fault).
Si indichi anche, con un asterisco o altro simbolo, il frame/pagina in testa alla coda FIFO.
Indicare anche il numero totale di page faults.

Variante con 5DB18

| Addr. | 1f040 | 3E320 | 5DB18 | 6C6E9 | 2A8F3 | 6D98C | 13AB2 | 240AB | 94B0C | 12AF1 |
|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|-------|
| P#    | 7     | F     | 17    | 1B    | A     | 1B    | 4     | 9     | 25    | 4     | 
| F1    | 7_0   | 7_0   | 7_0*  | 1B_0  | 1B_0  | 1B_1  | 1B_1* | 1B_0  | 1B_0* | 4_0   | 
| F2    |       | F_0   | F_0   | F_0*  | A_0   | A_0   | A_0   | 9_0   | 9_0   | 9_0*  | 
| F3    |       |       | 17_0  | 17_0  | 17_0* | 17_0* | 4_0   | 4_0*  | 25_0  | 25_0  | 
| PF    | X     | X     | X     | X     | X     |       | X     | X     | X     | X     |

Total Page Faults = 9

Variante con 1DB18

| Addr. | 1f040 | 3E320 | 1DB18 | 6C6E9 | 2A8F3 | 6D98C | 13AB2 | 240AB | 94B0C | 12AF1 |
|-------|-------|-------|-------|-------|-------|------|-------|-------|-------|------|
| P#    | 7     | F     | 7     | 1B    | A     | 1B   | 4     | 9     | 25    | 4    | 
| F1    | 7_0   | 7_0   | 7_1   | 7_1*  | 7_0   | 7_0  | 4_0   | 4_0   | 4_0*  | 4_1  | 
| F2    |       | F_0   | F_0   | F_0   | A_0   | A_0  | A_0*  | 9_0   | 9_0   | 9_0  | 
| F3    |       |       |       | 1B_0  | 1B_0* | 1B_1* | 1B_0  | 1B_0* | 25_0  | 25_0 | 
| PF    | X     | X     |       | X     | X     |      | X     | X     | X     |      |

Total Page Faults = 7

**Svolgimento**

Per calcolare i riferimenti, dividiamo l'indirizzo per la dimensione della pagina:

Numero_Pagina = Indirizzo / Dimensione_Pagina

Ad es. = R 1F040: 0x1F040 ÷ 0x4000 = 7 (resto 0x3040)
Pagina: 7

oppure fare lo shift a destra di 14 bit (dividendo per 0x4000)

---