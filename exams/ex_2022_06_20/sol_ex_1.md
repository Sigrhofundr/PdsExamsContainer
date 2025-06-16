## Domanda 1

Considerare un file system Unix-like, basato su inode, con 13 puntatori / indici (10 diretti, 1 singolo
indiretto, 1 doppio indiretto e 1 triplo indiretto). I puntatori / indici hanno una dimensione di 32 bit e i
blocchi del disco hanno una dimensione di 2 KB. Il file system risiede su una partizione del disco di 800
GB, che _include sia blocchi di dati che blocchi di indice_.

A) Supponendo che tutti i metadati (eccetto i blocchi indice) abbiano dimensioni trascurabili,
calcolare il numero massimo di file che il file system puo ospitare, utilizzando l'indicizzazione
indiretta doppia (N2) e l'indicizzazione indiretta tripla (N3).

B) Dato un file binario di dimensione 10240.5KB, calcolare esattamente quanti blocchi indice e
blocchi di data occupa il file.

C) Considerare lo stesso file della domanda B, su cui viene chiamata l'operazione lseek (fd, offset,
SEEK_END) per posizionare l'offset del file per la successiva operazione di lettura / scrittura
(SEEK_END significa che l'offset viene calcolato dalla fine del file). Supponiamo che fd sia il
descrittore di file associato al file (gia aperto), e offset= $0x00800000$. Calcolare il numero di
blocco logico (numerazione relativa ai blocchi del file, numerati a partire da 0) in corrispondenza
del quale viene spostata la posizione (da lseek). Se il file utilizza un'indicizzazione indiretta singola
(o doppia / tripla), calcolare quale riga del blocco di indici esterno contiene l'indice del blocco di
dati (o l'indice del blocco di indici piu interno).