# Registro del grafo — stato dei collegamenti

Questo file è **stato**, non istruzioni: vive fuori dal `SKILL.md` perché si riscrive a ogni aggiornamento, mentre le istruzioni devono restare stabili. Si carica solo quando serve (aggiornamento, censimento, rinomina), non a ogni attivazione della skill.

Le righe sotto sono un **template con esempi**: alla prima esecuzione la regola di censimento le sostituisce con le skill reali del tuo sistema, e da lì il registro resta vivo — ogni aggiornamento di una skill aggiorna anche la sua riga.

| Skill | Sezione "Prossime skill" | Collegamenti in uscita noti | Note |
|---|---|---|---|
| risparmio-token | ☑ | skill-creator *(grigio)*, skill-linker, verifica-rigore *(grigio)* | nata con sezione standard |
| decodifica-intento | ☑ | anti-compiacimento, apprendimento-adattivo *(grigio)*, gestione-progetti *(grigio)* | pre-processing delle richieste, aggancio permanente via regola in memoria |
| analista-progetti | ☑ | orchestratore-pianifica, percorsi-di-sviluppo, verifica-rigore *(grigio)* | onestà ereditata da anti-compiacimento |
| sintesi-generativa | ◪ | quiz-exam-generator, apprendimento-adattivo | esempio di stato intermedio: testo pronto, in attesa di applicazione nell'editor dell'app |
| *(le tue skill…)* | ☐ | | il censimento aggiunge qui una riga per ogni skill trovata sul disco e assente dal registro |

Legenda: ☐ = da fare · ◪ = testo pronto, in attesa di applicazione nell'editor dell'app · ☑ = sezione standard presente · *(grigio)* = destinazione non presente nell'installazione — todo-list naturale del sistema, non errore.

Quando una skill viene aggiornata: spunta la casella, riporta i collegamenti effettivi nella terza colonna, marca *(grigio)* le destinazioni che non esistono ancora.
