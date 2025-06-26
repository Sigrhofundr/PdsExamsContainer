## Domanda 2

Considera un file system che utilizza l'allocazione concatenata (**linked allocation**) per l'archiviazione dei file.
Il disco è diviso in blocchi di 4 KB ciascuno. Ogni blocco contiene un puntatore al blocco successivo, che occupa 4 byte del blocco.
Un file richiede 5 MB di spazio di archiviazione.

A. Calcola il numero di blocchi disco richiesti per memorizzare l'intero file, incluso lo spazio necessario per i puntatori.<br>
B. Se i puntatori fossero ridotti a 2 byte (assumendo una capacità del disco minore), ricalcola il numero di blocchi richiesti.
Questa modifica renderebbe più efficiente la memorizzazione del file in termini di utilizzo dei blocchi?<br>
C. Confronta l'overhead introdotto dai puntatori in entrambi i casi come percentuale dello storage totale utilizzato.

---

### Risposte

>[!NOTE]
> In questo esercizio, nella parte A i miei risultati differiscono (di poco)


A. Ci dobbiamo ricavare il numero preciso di bytes → Dim file = 5 MB = 5 * 1024 * 1024 = 5.242.880; ogni blocco = 4KB = 4096 B.<br> <!-- Nella soluzione del prof c'è un *1024 in più-->
Dato che all'inizio del blocco vi sono 4 B di puntatori, il blocco = 4096-4 = 4092<br>
N_Blocchi_disco = DIM_file/DIM_Singolo_blocco = ceiling(5242880/4092) =  ceiling(1281.25) → 1282 blocchi (per puntatori a 4 byte)

B. Dim_blocco = 4096 -2 = 4094 B => N_Blocchi_disco = DIM_file/DIM_Singolo_blocco = ceiling(5242880/4094) = ceiling(1280.62) => 1281 blocchi (si risparmia un blocco)

C. Nel I caso: <br>
* Overhead I = 4* 1282 = 5128 bytes; quindi la percentuale → Overhead I = 5128/5242880 * 100 = 0.0978%;
* Overhead II = 2 * 1281 = 2562 Bytes ; percentuale → Overhead II = 2562/5242880 * 100 = 0.0488 %;

quindi l'overhead è quasi dimezzato quando i usa un puntatore a 2B
