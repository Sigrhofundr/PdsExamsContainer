## Domanda 1

Sia dato un sistema di memoria virtuale con paginazione, nel quale vengono indirizzati i Byte.
Il sistema dispone di TLB (Translation Look-aside Buffer), su cui si misura sperimentalmente un "hit ratio" del 90%.
La tabella delle pagine ("page-table") viene realizzata con uno schema a due livelli, nel quale un indirizzo logico di
64 bit viene suddiviso (da MSB a LSB) in 3 parti: p1, p2 e d, rispettivamente di 40 bit, 12 bit e 12 bit.
Non si utilizzano ulteriori strutture dati (quali tabelle di hash o inverted page table) per velocizzare gli accessi.
La memoria virtuale viene gestita con paginazione a richiesta.

Si risponda alle seguenti domande:

A) **D** - Supponendo che non sia presente una memoria cache, e che la memoria RAM abbia tempo di accesso di 200 ns,
si calcoli il tempo effettivo di accesso (EAT) per il caso proposto (TLB hit ratio = 90%), assumendo che il tempo di
accesso alla TLB sia trascurabile.<br>
**R** - Riepilogando: indirizzi : 64 bit = 8 B; **NB** Bisogna considerare che in caso di TLB_MISS, il lookup richiede un totale di 3 accessi! (doppio livello tabella indirizzi + ram)<br>
EAT = hit_rate_TLB * T_acc_RAM + (1-hit_rate_TLB) * 3 * T_acc_RAM = 0.9 * 200 * 10^-9 + (3 * 0.10 * 200 * 10^-9) =
= 1.8 * 10^-7 + 6 * 10^-8 = 2.4 * 10^-7 = 240 ns

B) **D** - Si vuole ora tener conto del fatto che il sistema dispone di memoria cache, per la quale si sa che la probabilità
di (cache) miss è del 10% (un accesso non in cache ogni 10) e che la cache ha tempo di accesso 30 ns. Si ricalcoli EAT,
con gli stessi dati per quanto riguarda la TLB e ipotizzando, nel caso di cache miss, un tempo di accesso alla RAM di 230 ns.<br>
**R** - Dobbiamo modificare la formula originale:<br>
T_acc_RAM = (1-hit_cache)*T_acc_RAM_cache + Hit_cache * T_acc_cache = 0.1 * 230 ns + 0.9 * 30 ns = 50 ns;
EAT = 1.2 * T_acc_RAM = 60 ns
<!-- NON l'ho capito a fondo -->
<!-- EAT = (hit_rate_TLB_no_cache_miss * T_acc_RAM) + (hit_rate_TLB_cache_miss * T_acc_RAM_cache_miss) + (1-hit_rate_TLB) * 3 * T_acc_RAM =
= (0.81 * 200 * 10^-9)+ (0.09 * 230 * 10^-9) +(3 * 0.10 * 200 * 10^-9) =  2,427 * 10^-7 = 242,7 ns--> <br>

C) **D** - Si consideri ora la frequenza di page fault p. Si faccia riferimento al caso della domanda B (presenza di cache)
e si ipotizzi che un page fault sia servito in 5 ms. Quanto deve valere p affinché si possa garantire un degrado
massimo del 25% per EAT (causato sia dalla TLB che dai page fault)? Si tratta di un valore massimo o minimo per p?<br>
**R** - T_PF = 5ms <br>
EAT_PF = (1-p) * EAT_PT + p*T_PF = (1-p) * 60 + p*5*10^6 ns => <br>
=> EAT_PF<= 1.25 * T_RAM = 75 ns

(1-p)*60 + p*5*10^6 <= 75 <br>
(5*10^6 - 60) *p <= 75 -60 <br>
(5*10^6)*p <= 15 (rimuovendo la parte trascurabile) <br>
p <= 15/5*10^-6 = 3*10^-6

Si tratta di valor massimo, vista la disequazione, osservando che al crescere della probabilità di PF aumenta EAT_PF che deve
essere inferiore al limite massimo.
