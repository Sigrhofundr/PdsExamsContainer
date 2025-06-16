## Domanda 1

**IMPORTANTE:** Se i risultati sono numeri, riportare passaggi intermedi rilevanti e/o formule usate. Le risposte SI/NO vanno motivate.

Sia dato un sistema di memoria virtuale con paginazione a richiesta, nel quale vengono indirizzati i Byte.<br>
Il sistema ha indirizzi su 32 bit, con pagine di 4KB, e dispone di TLB (Translation Look-aside Buffer). La tabella delle pagine ("page-table") è a un solo livello.<br>
Non si utilizzano ulteriori strutture dati (quali tabelle di hash o inverted page table) per velocizzare gli accessi. Un TLB miss viene gestito facendo accesso alla tabella delle pagine,
gestendo l'eventuale PF, aggiornando la TLB, e ripetendo l'accesso in memoria con TLB aggiornata.<br>
Si sa che la memoria RAM ha tempo di accesso di 100 ns, che un Page Fault ha un costo di 2.1ms oppure 4.1ms, a seconda
del valore del modify bit relativo alla pagina scelta come vittima. Si sono valutate tre stringhe di riferimento w1, w2 e w3,
di lunghezza 100000 (ognuna). In w1 si osservano 10000 TLB miss e nessun PF, in w2 20000 TLB miss e 10 PF (tutti con modify bit 0),
in w3 10000 TLB miss e 20 PF, di cui 10 con modify bit 0 e 10 con modify bit 1. Si sa inoltre che le tre stringhe rappresentano
il comportamento del sistema in esame, assumendo che w2 e w3 siano equiprobabili e con probabilità (sia w2 che w3) doppia rispetto a w1.

Si risponda alle seguenti domande:

A) **D** - Si calcoli il numero complessivo di accessi alla TLB (compresi gli accessi ripetuti dopo un miss) eseguendo, in sequenza, le tre stringhe di riferimento.<br>
Si calcoli quindi, ESCLUDENDO I TEMPI DI PF, il tempo effettivo di accesso in RAM, dapprima per la sequenza delle tre stringhe e poi per il sistema che rappresentano (tenendo conto delle relative probabilità).<br>
**R** - Riepilogando i dati: <br>
Indirizzi 32 bit -> 4 B; DIM_Pagine 4 KB -> 4 * 1024 B = 4096 B = 2^12; (un livello => 12 bit page number - 20 offset?)<br>
Lunghezza_stringa_w1=Lunghezza_stringa_w2=Lunghezza_stringa_w3 = 100.000;<br>
TLB_miss_w1 = 10.000 ; PF_w1 = 0;<br>
TLB_miss_w2 = 20.000 ; PF_w2 = 10; (modify bit 0)<br>
TLB_miss_w3 = 10.000 ; PF_w3 = 20; (10 con mod_bit 0 e 10 con mod_bit 1)<br>
Probabilità: P(w2) = P(w3) = 2*P(w1)<br>
T_accesso_ram = 100ns; PF_mb_1 = 4.1 ms; PF_mb_0 = 2.1 ms<br>
<br>

Accessi_TLB_TOT = Lunghezza_stringa + TLB_Miss<br>
Accessi alla RAM (escludendo PF): per TLB hit: 1 accesso RAM (dato); per TLB Miss: 2 accessi RAM (Page table + dato)<br>
Tempo accesso effettivo per una stringa:<br>


Effective Access Time (EAT) nei sistemi con TLB = hit_rate_TLB * T_acc_RAM + (1-hit_rate_TLB) * 2 * T_acc_RAM<br>
dove:<br>
- hit_rate_TLB * T_acc_RAM : rappresenta il costo di quando si verifica il TLB hit; se è hit sarà necessario solo un accesso alla RAM, quindi sarà uguale a T_acc_RAM;
- (1-hit_rate_TLB) * 2 * T_acc_RAM corrisponde al costo della TLB_miss, cioè il caso in cui l'elemento non è nella TLB; <br>
  la h_TLB sarà dunque data da: h_TLB = Numero_TLB_hit / Num_totale_acc_mem_logici<br>
  dove:<br>
  Numero_TLB_hit = Lunghezza della stringa − Numero di TLB Miss
  Stringa w1:<br>
- TLB_hit_w1 = 100.000 - 10.000 = 90.000;<br>
- h_TLB_w1 = 90.000/100.000 = 0,9;
- EAT_w1 = (0.9 * 100 * 10^-9) + (1-0.9)*2*(10^-9) = 1,1 * 10^-7 s = 110 ns;
  Stringa w2: <br>
- TLB_hit_w2 = 100.000 - 20.000 = 80.000;<br>
- h_TLB_w2 = 80.000/100.000 = 0,8;
- EAT_w2 = (0.8 * 100 * 10^-9) + (1-0.8)*2*(10^-9) = 1,2 * 10^-7 s = 120 ns;
  Stringa w3:<br>
  visto che non consideriamo i PF sarà uguale a w1 => EAT_w1 = 110 ns;


T_acc_stringa_wi = EAT_wi * lunghezza stringa<br>
Per calcolare gli accessi TLB totali, sommiamo la lunghezza della stringa ai TLB miss (per i riaccessi dopo miss).<br>
Per il tempo di accesso RAM, consideriamo che ogni TLB hit richiede 1 accesso RAM e ogni TLB miss ne richiede 2.<br>
Il tempo medio del sistema si calcola pesando i tempi di ogni stringa con le rispettive probabilità.<br>
Accessi_TLB_TOT = Lunghezza_stringa + TLB_Miss = (sommatoria delle singole stringhe)<br>
Accessi_TLB_TOT = (100.000 + 10.000) + (100.000 + 20.000) + (100.000+ 10.000) = 340.000 accessi<br>
Stringa w1:<br>
- T_acc_w1 = (110 * 10^-9) * 100.000 = 11 * 10^-3 s = 11 ms;<br>
Stringa w2: <br>
- T_acc_w2 = (120 * 10^-9) * 100.000 = 12 * 10^-3 s = 12 ms;<br>
Stringa w3:<br>
- T_acc_w3 = T_acc_w1 = 11 ms;<br>

Tempo_effettivo_accesso_in_ram_stringhe = T_w1 + T_w2 + T_w3 = (11 +12 +11) * 10^-3 = 34ms;<br>
da Probabilita: P(w2) = P(w3) = 2*P(w1)<br> mi ricavo , per sostituzione:<br>
P(w1) = p; P(w2) = 2p; P(w3) = 2p; => TOT = p+2p+2p = 5p => p = 1/5 => P(w1) = 1/5; P(w2) = P(w3) = 2/5<br>

Mi ricavo ora l'EAT medio ponderato per il sistema
EAT_sys = (P(w1) * EAT_w1) + (P(w2) * EAT_w2) + (P(w3) * EAT_w3) =
= (1/5×110 ns)+(2/5×120 ns)+(2/5×110 ns) = 22 ns + 48 ns+ 44 ns = 114 ns

B) Si considerino ora i PF. Si spieghi brevemente come va correlato il modify bit con i tempi di gestione dei PF. Si calcoli, per ognuna delle tre stringhe di riferimento e quindi per il sistema che rappresentano, il tempo effettivo di accesso in RAM, tenendo conto sia della TLB che dei PF.<br>
Innanzitutto, il MB è posto a 0 se la pagina in memoria è identica alla copia sul disco (page clean), 1 se è stata modificata in memoria (page dirty);<br>
La correlazione con PF sarà data da : <br>
1. Selezionare la pagina vittima;
2. - se PF ha MB = 0 => Non serve salvare la pagina vittima (è giù uguale su disco);
- se PF ha MB = 1 => Salva pagina vittima su disco (write-back);
3. Carica nuova pagina dal disco;
4. Aggiorna page table e TLB : -> se MB 0 -> 2.1 ms, se MB 1 -> 4.1 ms

Tempo totale per stringa (con PF) => T_tot = T_accessi_normali + T_PF;<br>
dove<br>
T_acc_norm = (TLB_hit × t_RAM) + (TLB_miss × 2 × t_RAM);<br>
T_pf = (PF_clean × t_PF_clean) + (PF_dirty × t_PF_dirty)<br>
dove <br>
t_pf_clean = 2.1 ms; t_pf_dirty = 4.1ms <br>
T_pf_w1 = 0 (non ci sono PF)<br>
T_pf_w2 = (10 * 2.1 ms) + 0= 21 ms<br>
T_pf_w3 = (10 * 2.1 ms) + (10 * 4.1) = 21 ms + 41 ms = 62 ms<br>

Riprendendo i tempi calcolati in precedenza: <br>
T_acc_tot_w1 = 11 ms + 0 = 11 ms<br>
T_acc_tot_w2 = 12 ms + 21 ms = 33 ms<br>
T_acc_tot_w3 = 11 ms + 62 ms = 73 ms<br>

Ricalcoliamo il numeratore per il tempo medio: <br>
P_w1 * T_acc_tot_w1 = 1/5 * 11 * 10 ^-3 = 2.2 ms <br>
P_w2 * T_acc_tot_w2 = 2/5 * 33 * 10 ^-3 = 13.2 ms<br>
P_w3 * T_acc_tot_w3 = 2/5 * 73 * 10 ^-3 = 29.2 ms<br>

Nel denominatore useremo la lunghezza della stringa (100.000) perché vogliamo il tempo medio per riferimento alla memoria, non per accesso fisico.
T_medio_con_PF = (44.6 * 10^-3)/(P_w1 × 100.000 + P_w2 × 100.000 + P_w3 × 100.000) = (44.6 * 10^-3)/100.000 = 4,46 * 10^-7 = 446 nanosecondi<br>