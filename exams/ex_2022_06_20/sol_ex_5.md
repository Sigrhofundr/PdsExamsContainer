## Domanda 5

Sia data un sistema di memoria virtuale con paginazione, nel quale vengono indirizzati i Byte. II sistema
dispone di TLB (Translation Look-aside Buffer). La tabella delle pagine ("page-table") viene realizzata con
uno schema a due livelli, nel quale un indirizzo logico di 64 bit viene suddiviso (da MSB a LSB) in 3
parti: p1, p2 e d, p2 ha 14 bit e d 13 bit. Non si utilizzano ulteriori strutture dati (quali tabelle di hash o
inverted page table) per velocizzare gli accessi. La memoria virtuale viene gestita con paginazione a
richiesta.

Si risponda alle seguenti domande:

A) Supponendo che la memoria RAM abbia tempo di accesso di 75 ns, si ca/coli ii TLB miss ratio
(frequenza di miss), sapendo che, a causa dei TLB miss, si sa che $EAT_{PT}>= 100 ns$,
assumendo che ii tempo di accesso al/a TLB sia trascurabile.<br>
Si tratta di un lower bound o di un upper bound per la TLB miss ratio?

B) Si consideri ora la frequenza di page fault $p_{PF}$· Una valutazione sperimentale mostra che un
page fault viene servito in 2 ms in media, e che ii degrado di prestazioni (aumento di tempo di
esecuzione) causato dai page fault e compreso tra 10% e 30%. Calcolare l'intervallo di valori
di $p_{PF}$ compatibile con la valutazione sperimentale, assumendo $EAT_{PT} = 100 ns$.

C) Una seconda valutazione sperimentale (una simulazione) viene fatta usando due stringhe di
riferimento $w_1$, $w_2$, aventi lunghezza, rispettivamente, $len(w_1)=10^6$, $len(w_2)=2*10^6$ e probabilità
di stringa $p_1$ e $p_2$. Le simulazioni hanno generato 400 page fault in totale (100 su $w_1$ e 300 su
$w_2$) e stimato una probabilità empirica f = 0.00012 (frequenza attesa) che avvenga un page
fault net sistema reale. Si calcolino i valori di $p_1$ e $p_2$
