## Domanda 2

Un processo user vuole effettuare un input da un dispositivo di IO a caratteri (character device)
mediante una strategia basata su polling del dispositivo. Per evitare un'attesa troppo lunga (nel
loop di polling) da parte del driver del dispositivo, si chiede all'autore del programma eseguito dal
processo user, di fare direttamente polling mediante un loop di lettura del registro di stato del
dispositivo.

A) È possibile effettuare questa operazione? (se si, dire come, se no, dire perche)

B) Supponendo che il dispositivo sia associato alla tastiera e lo si gestisca in interrupt, ci si
aspetta che venga generato un interrupt per ogni carattere ricevuto oppure un interrupt per
ogni "invio" (cioè enter, fine-riga)? (motivare)

Dato un altro dispositivo di 10, gestito a blocchi:

C) È possibile che una sola scrittura (mediante chiamata a _write()_), tenti di fare output di piu di
una coppia puntatore/dimensione?

D) Dato il driver che realizza la scrittura, supponendo che il sistema sia dotato di OMA
controller, e possibile che venga eseguita "prima" una operazione di lettura e/o scrittura
attivata successivamente a questa, da un altro processo? E (stessa domanda) dallo
stesso processo? (motivare)