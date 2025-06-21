## Domanda 5

Sia data un sistema di memoria virtuale con paginazione, nel quale vengono indirizzati i Byte. II sistema
dispone di TLB (Translation Look-aside Buffer). La tabella delle pagine ("page-table") viene realizzata con
uno schema a due livelli, nel quale un indirizzo logico di 64 bit viene suddiviso (da MSB a LSB) in 3
parti: p1, p2 e d; p2 ha 14 bit e d 13 bit. Non si utilizzano ulteriori strutture dati (quali tabelle di hash o
inverted page table) per velocizzare gli accessi. La memoria virtuale viene gestita con paginazione a
richiesta.

Si risponda alle seguenti domande:

A) Supponendo che la memoria RAM abbia tempo di accesso di 75 ns, si calcoli ii TLB miss ratio
(frequenza di miss), sapendo che, a causa dei TLB miss, si sa che $EAT_{PT}>= 100 ns$,
assumendo che ii tempo di accesso alla TLB sia trascurabile.<br>
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


**Soluzione**

Riepilogando intanto la premessa:<br>
* schema a due livelli;
* indirizzo logico 64 bit -> 8 Byte
* è formato da p1, p2 e d: p1 = 37 ,p2 = 14, d = 13;
* da ciò ci ricaviamo che la Dim pagina = 2^13 = 8192 byte => 8 KB;

---

A) Ci dice che:
* T_acc_RAM = 75 ns;
* complessivamente, EAT_PT >= 100 ns;
* TLB trascurabile (accesso alla tabella TLB);

Dato che EAT = hit_ratio_TLB * T_acc_RAM + hit_ratio_TLB_Miss * 3 (due livelli + ram) * t_acc_RAM

da questa mi ricavo

100 ns <= hit_ratio_TLB * 75 ns + (1 - hit_ratio_tlb) * 3 * 75 ns <br>
100 ns <= 75ns hit_ratio_TLB + 225 ns - 225ns hit_ratio_tlb <br>
100 ns - 225 ns < = -150 ns hit_ratio_tlb
- 125 <= -150 ns hit_ratio_tlb
125 > 150 hit_ratio_tlb
hit_ratio_tlb < 0,833

ci interessava ricavarci il tlb_miss
hit_ratio_TLB_miss = 1 - hit_ratio_tlb = 0,166666667
miss_ratio_tlb >= 16,7  % ed è un lower bound

---

B) ci dice:
* PF servito in 2 ms;
* degrado tra 10 e 30 %;
* assumendo EAT_PT = 100 ns;

sapendo che
EAT_TOT = EAT_no_PF * (1 - p_pF) + EAT_con_PF * p_PF
dove 
* EAT_senza_PF = tempo quando non c'è page fault
* EAT_con_PF = tempo quando c'è page fault
* p_PF = probabilità di page fault

EAT_con_PF = T_servizio_PF + EAT_PT

T_servizio_pf = tempo per servire PF dato dal testo = 2 ms
EAT_PT = tempo accesso dato, 100 ns

EAT_PF = 2ms + 100 ns = 2 * 10^-3 + 100 * 10^-9 = 2,0001 ms

Degrado = (EAT_TOT - EAT_PT) / EAT_PT * 100

10<= (EAT_TOT - EAT_PT) / EAT_PT * 100 <= 30

10 ≤ (EAT_TOT - 100) / 100 × 100 ≤ 30
10 ≤ (EAT_TOT - 100) ≤ 30

svolgiamo le due disequazioni, EAT_PT l'abbiamo, quindi
10 ≤ (EAT_TOT - 100)
EAT_TOT >= 110

(EAT_TOT - 100) ≤ 30
EAT_TOT <= 130

Riprendendo la formula

EAT_TOT = EAT_no_PF * (1 - p_pF) + EAT_con_PF * p_PF

considerato che EAT_no_PF = EAT_PT dato = 100 ns
vediamo il range di pf, prima per 110

110 <= 100 ns - 100ns * p_PF + 2,000,100 ns * p_PF
110 - 100  <= (- 100ns + 2,000,100 ns)*p_PF
10 <= 2,000,000  p_PF
10/2,000,000  <= p_PF
p_PF_min >= (circa) 5*10^-3

sostituiamo adesso 130 per il limite sup

130 >= 100 ns - 100ns p_PF + 2,000,100 ns * p_PF
130 - 100 >= 2.000.000 p_PF
30/ 2.000.000 >= p_PF
p_PF_max < 1.5 * 10^-5

quindi avremo
$5×10^-6 ≤ p_PF ≤ 1.5×10^-5$

Il risultato ha senso perché:

I page fault sono 20,000 volte più costosi degli accessi normali
Anche probabilità minuscole hanno impatti drastici

---

C) ci viene detto che

* len_w1 = 10^6; genera 100 pf; probabilità p_1
* len_w2 = 2*len_w1 = 2*10^6; genera 300 pf; probabilità p_2

Frequenza di Page Fault per stringa
* freq_w1 = 100 / 10^6 = 10^-4
* freq_w2 = 300 / 2*10^6 = 1,5 * 10^-4

dato che
p_1 + p_2 = 1

f = p1* freq_1 + p2* fre1_2

0.00012 = p₁ × 10⁻⁴ + p₂ × 1.5×10⁻⁴

dobbiamo risolvere le due equazioni

p_1 + p_2 = 1
0.00012 = p₁ × 10⁻⁴ + p₂ × 1.5×10⁻⁴

sostituiamo p2 = 1-p1<br>
0.00012 = p_1 * 10^-4 + ((1-p1)*1.5*10^-4)<br>
0.00012 = p_1 * 10^-4 + ((1.5 * 10^-4)  -p1 *1,5*10^-4)<br>
0.00012 - 1,5 * 10^-4= p_1*10^-4 - 1.5p1 * 10^-4<br>
-3 * 10^-5 = - 0.5 * p1 * 10^-4<br>
p_1 = 0.6<br>
da cui<br>
p_2 = 0.4


