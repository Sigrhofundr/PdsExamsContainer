## Domanda 3

**IMPORTANTE:** SE I RISULTATI SONO NUMERI, RIPORTARE PASSAGGI INTERMEDI RILEVANTI E/O FORMULE USATE. LE RISPOSTE SI/NO VANNO MOTIVATE.

Si consideri un contesto di paginazione a richiesta con sostituzione di pagine. Si risponda alle domande seguenti.

A) Spiegare brevemente per quale/i scopo/i possono essere usati, nella Page Table, i bit denominati:
1. valid bit (Bit di Validità)

   Scopo: Indica se la pagina logica è attualmente presente in memoria fisica (RAM).
   Funzionamento: Se 1, la pagina è in RAM e l'indirizzo nel campo frame è valido. Se 0, la pagina non è in RAM e un accesso ad essa causa un Page Fault.
   Utilizzo: Fondamentale per la gestione della paginazione a richiesta e per scatenare i Page Fault quando una pagina non è residente.

2. reference bit (Bit di Riferimento o Accesso)

   Scopo: Registra se la pagina è stata acceduta (letta o scritta) di recente dall'hardware.
   Funzionamento: Inizializzato a 0, viene impostato a 1 dall'hardware al primo accesso alla pagina.
   Utilizzo: Cruciale per gli algoritmi di sostituzione pagina basati sull'uso recente, come il Second-Chance (Clock), per identificare le pagine meno usate.

3. modify bit (Bit di Modifica o "Dirty Bit")

   Scopo: Indica se la pagina in memoria fisica è stata modificata (scritta) da quando è stata caricata.
   Funzionamento: Inizializzato a 0 al caricamento, impostato a 1 dall'hardware in caso di scrittura.
   Utilizzo: Essenziale negli algoritmi di sostituzione pagina per l'ottimizzazione: se 0, la pagina vittima non deve essere scritta su disco. Se 1, deve essere scritta (costoso) prima di essere rimpiazzata, poiché la copia in RAM è diversa da quella su disco. Utilizzato anche in algoritmi avanzati (es. Enhanced Clock) in combinazione col reference bit.
   B) Si tratta di bit presenti solo nella PT o anche nella TLB? (motivare).<br>
   **R** Nella TLB troviamo il valid e il modified bit, per i motivi già citati in precedenza.
   C) **D** - Si vuole realizzare una Page Table a un solo livello unica per tutti i processi. È possibile?  Se no, spiegare perché. Se sì, dire quale è il costo di una conversione pagina-frame.<br>
   **R** - In un contesto di sistemi operativi moderni che garantiscono l'isolamento dei processi e la sicurezza, una tale configurazione non è possibile. La fattibilità dipende fortemente dallo scopo e dall'architettura hardware specifica.<br>
   Le ragioni principali sono:
- Isolamento dei processi e protezione: Ogni processo in un sistema operativo moderno ha il proprio spazio di indirizzamento logico virtuale.
  Una PT unica per tutti i processi violerebbe completamente questo isolamento. Ogni processo potrebbe accedere (e potenzialmente modificare)
  la memoria di qualsiasi altro processo, creando gravi problemi di sicurezza, stabilità e privacy. I dati e il codice dei processi non sarebbero protetti.
- Gestione della memoria: Ogni processo ha la propria visione della memoria, con le proprie pagine mappate a frame fisici. Una PT unica implicherebbe che tutti i
  processi condividano lo stesso identico spazio di indirizzamento virtuale e le stesse mappature, cosa che non è compatibile con la gestione di più processi concorrenti e
  indipendenti. Le pagine di un processo potrebbero essere sovrascritte o confuse con quelle di un altro.
- Ambito degli indirizzi virtuali: In un sistema con più processi, indirizzi virtuali identici in processi diversi devono
  mappare a indirizzi fisici diversi. Una PT unica non permetterebbe questa distinzione in modo efficiente e sicuro.

Costo di una conversione pagina-frame (se fosse possibile):

Se, ipoteticamente e in un contesto non realistico di sistema moderno, una tale configurazione fosse implementabile (ad esempio in un sistema monoprocesso o con forte cooperazione e senza requisiti di sicurezza):

Il costo di una conversione pagina-frame (senza considerare la TLB) sarebbe di un singolo accesso alla RAM.
Questo perché la Page Table a un solo livello risiede in RAM e ogni ricerca di traduzione richiederebbe di accedere all'entrata appropriata della Page Table per ottenere il numero di frame fisico.
