## Domanda 2: Hard Disk Drive (HDD)

Considera un Hard Disk Drive (HDD) con tecnologia e geometria standard. Il disco ha:
* dimensione del settore di 512 Byte,
* 2048 tracce per superficie,
* 50 settori per traccia,
* 5 piatti a doppia faccia,
* tempo medio di seek di 10 msec.

A) **D** - Qual è la capacità di una traccia (in Byte)? Qual è la capacità di ogni superficie? Qual è la capacità del disco? Quanti cilindri ha il disco?<br>
**R** - Dati per traccia = num settori per traccia * dimensione del settore = 50 * 512 = 25600 B = 25 KB;
<br> Dati per superficie = dati per traccia * tracce per superficie = 52.428.800 B = 50 MB; <br>
Dati per disco = Dati per superficie * (numero dischi * lati_dischi) = 52428800 * (5*2) = 524.288.000 B = 500MB<br>
Numero cilindri = Numero tracce per superficie = 2048 cilindri <br>
B) **D** - Il metodo di indirizzamento del disco CHR (Cilindro-Testina-Settore) consente a un blocco disco di estendersi su più settori di una data traccia (identificata da una coppia cilindro, testina), ma non su tracce diverse. I seguenti sono formati di blocco validi: 256 B, 2048 B, 51200 B? (Motivare le risposte)<br>
**R** - Date le premesse: un blocco può attraversare più settori della stessa traccia e un blocco NON può attraversare tracce diverse. Un blocco per essere valido deve essere multiplo della dimensione del settore e non deve superare la capacità complessiva della traccia.<br>
Quindi: 256 B? NO, non è multiplo di 512; 2048 B ? sì, è multiplo e corrisponde a 4 settori consecutivi; 51200? NO, supera la capacità per traccia!
C) **D** - Se i piatti del disco ruotano a 5400 rpm (rivoluzioni al minuto), qual è il ritardo massimo di rotazione? Qual è il ritardo medio di rotazione?
Se una traccia di dati può essere trasferita per rivoluzione, qual è il tasso di trasferimento (espresso in bit/secondo)?
Quale sarebbe il tasso di trasferimento se un intero cilindro di dati potesse essere trasferito per rivoluzione?<br>
**R* - Rivoluzioni per secondo = RPM / 60 = 5400 / 60 = 90 RPs; Tempo di rivoluzione = 60 secondi / RPM = 60 / 5400 = 0.0111111111 sec => circa 11ms <br>
ritardo_max = tempo_riv = 11 ms; Ritardo medio = tempo_riv /2 = 5.5 ms
Bit_per_traccia = capacità_traccia * 8 = 25600 B * 8 = 204800 bit <br>
Tasso_trasferimento_traccia = bit_per_traccia / tempo_rivoluzione = 204800 / 0.0111111 = 18.432.000 bit al secondo<br>
capacità cilindro = bit_per_traccia * num_superfici = 25600 * 10 = 256.000 B<br>
bit_per_cilindro = capacità_cilindro * 8 = 2048000 bit<br>
tasso_trasferimento_cilindro = bit_per_cilindro/ tempo_rivol = 184.320.000 bit/sec<br>
<!--Non ci serve: throughput massimo = Dati per traccia * rivoluzioni per secondo = 25600 * 90 = 2.304.000;--> <br>