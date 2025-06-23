## Domanda 2

A. Sia dato un disco organizzato con struttura RAID. Sono dati il tempo medio tra guasti (MTTF: Mean Time To Failure),
il tempo medio per riparazione (MTTR: Mean Time To Repair), e il tempo medio tra perdite di dato (MTTDL: Mean Time To Data Loss).
A causa di vincoli aggiuntivi, la probabilità di guasto di un disco, mentre l'altro è in riparazione, viene RADDOPPIATA.

Si può considerare che questo sia equivalente a:

A1. moltiplicare MTTR per 2?

A2. moltiplicare MTTR per 0.5?

A3. Lasciare tutto com'è (senza modifiche) in quanto MTTR è trascurabile?

B. Si consideri una struttura RAID con 2 dischi in configurazione “mirrored”.
Se ognuno dei dischi può guastarsi in modo indipendente dall’altro, con MTTR = 36 ore, quanto deve essere MTTF per garantire MTTDL > 100*MTTF)?

---

**Risposte**

Riepiloghiamo le formule:
* $MTTF_{1°disk fail} = MTTF/Number_{disks}$
* $Prob_{2°fail during repair} = MTTR/MTTF$ 
* $MTTDL(Mean time to data loss) = MTTF_(1st_fail+2nd_fail_during_repair)$
* $MTTF_(fail+fail during repair) = MTTF_{1st fail} /Prob_{2nd fail during repair} = MTTF^2/(2*MTTR)$ 

A.1 Se ci poniamo come condizione che la struttura sia a due dischi, per come è strutturata la formula della probabilità,
se moltiplichiamo MTTR * 2 avremo effettivamente una probabilità aumentata del doppio

Esempio concreto
```
Supponendo MTTF (di base) sia 100.000 ore, e MTTR 10 ore, di base la Prob = 10 / 100* 10^3 = 1*10^-4
se moltiplicassimo per 2 MTTR => Prob = 2* 10/100.000 = 2*10^-4
Quindi la probabilita effettivamente raddoppia
```

A2. Per lo stesso motivo riportato nella domanda precedente, moltiplicare per 0.5 dimezzerebbe la probabilità, quindi NO.

A3. No, non è corretto lasciare tutto com’è e trascurare MTTR: anche se MTTR è piccolo rispetto a MTTF, 
ha un impatto diretto sulla probabilità di perdita dati. Trascurare MTTR significherebbe assumere che la finestra di 
rischio sia nulla, il che non è realistico. È invece fondamentale ridurre MTTR il più possibile, ma mai ignorarlo nei calcoli di affidabilità.

---

B. Dati forniti:
* MTTR = 36 ore
* cerchiamo MTTF per cui MTTDL > 100*MTTF

da questa seconda formula ci ricaviamo che
* MTTF < MTTDL/100

ma MTTDL = $MTTDL(Mean time to data loss) = MTTF_(1st_fail+2nd_fail_during_repair)$
che a sua volta = MTTF^2/(2*MTTR)

quindi sostituiamo
$MTTF < MTTF^2/(2*MTTR)*100$ => $2*MTTR*100 < MTTF^2 / MTTF$ => $MTTF > 2*MTTR*100$ => $MTTF > 2*36*100$ => $MTTF > 7200 ore$

[//]: # (* sapendo che in un configurazione a due dischi mirrored abbiamo:)

[//]: # (* $MTTF_{fail+fail during repair} = MTTF^2/&#40;2*MTTR&#41;$ sostituiamo nell'equazione)

[//]: # (* $MTTDL/100 > MTTF^2/&#40;2*MTTR&#41; => sqrt&#40;&#40;MTTDL * &#40;2*MTTR&#41;&#41;/100&#41; > MMTF)

