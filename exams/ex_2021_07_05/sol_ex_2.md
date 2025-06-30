### Domanda 2

A) Sia dato un disco organizzato con struttura RAID. Sono dati il tempo medio tra guasti (MTTF: Mean Time To Failure),
il tempo medio per riparazione (MTTR: Mean Time To Repair), e il tempo medio tra perdite di dato (MTTDL: Mean Time To Data Loss).<br>
E’ possibile che (motivare le risposte):<br>
A1) MTTR > MTTF?<br>
A2) MTTDL > 10 * MTTF?

B) Si consideri una struttura RAID con 2 dischi in configurazione “mirrored”. Se ognuno dei dischi può guastarsi in modo indipendente dall’altro,
con MTTF = 20000 ore, quanto deve essere MTTR per garantire MTTDL > 100*MTTF) ?

---

**Risposte**

A1) MTTR > MTTF **No** - (irrealistico il contrario) Il MTTR di solito è una grandezza nell'ordine delle decine di ore, l'MTTF invece in media è 
nell'ordine delle decine o centinaia di migliaia di ore;

A2) MTTDL> 10 * MTTF **Sì** - la relazione tra i due è data da MTTDL = MTTF(fail + fail during repair) = MTTF^2 /(N * MTTR) ,quindi dipenderà dal valore di MTTR. 
Lo scopo principale del RAID comunque è rendere molto grande MTTDL, rispetto a MTTF.

B1) dalla formula riportata prima, prendiamo il rapporto tra le grandezze (qui N = 2)
MTTDL = MTTF(fail + fail during repair) = MTTF^2 /(2 * MTTR) 
sostituiamo
* MTTF^2 / (2*MTTR) > 100 * MTTF
* MTTF * MTTF / 2 * MTTR > 100 * MTTF
* MTTF / 2* MTTR > 100
* MTTF > 200 MTTR
* MTTR < MTTF/200
* MTTR < 2 * 10^4 / 2 * 10^2
* MTTR < 100

Quindi la riparazione deve essere effettuata in meno di 100 ore, cioè poco più di 4 giorni

