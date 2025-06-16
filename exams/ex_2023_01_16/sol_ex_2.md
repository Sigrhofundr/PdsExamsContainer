## Domanda 2

Sia dato un file di dimensione 160MB, in un file system di dimensione complessiva 400GB. Si vogliono confrontare
le possibili organizzazioni di tale file, su file system con allocazione non contigua, secondo gli schemi:
* "linked list allocation"
* "File Allocation Table (FAT)"

Si supponga che i blocchi su disco (sia per i dati che per metadati) abbiano dimensione 8KB, e che i puntatori e
gli indici abbiano dimensione 32 bit.

Si dica, per ognuna delle 2 soluzioni:

A) Quanti blocchi (di dato e/o di metadato) sono necessari per il file (attenzione: Si richiede il conteggio ESATTO,
eventualmente espresso in termini di potenze di 2, ricordando che $1K=2^{10}$ e $1M=2^{20}$)?<br>
Nel caso della FAT, si dica quale percentuale di FAT è utilizzata per il file.

B) Quante letture in RAM e quanti accessi a disco (per trasferire un blocco in RAM) sono necessari
per leggere il byte n. 42070 all'interno del file? Si consideri un accesso diretto a file.
Si supponga di non utilizzare buffer cache, e che un puntatore o indice venga letto in RAM con una sola lettura.
Si supponga poi che il File Control Block (FCB) sia già in memoria RAM, che ogni accesso a disco legga o scriva un
blocco di 8KB, e che la FAT (qualora utilizzata) sia già in RAM.
