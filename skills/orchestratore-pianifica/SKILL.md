---
name: orchestratore-pianifica
description: This skill should be used when the user provides a concrete final project objective and asks to turn it into an executable task sequence — trigger phrases such as "pianifica", "fammi il piano", "scomponi questo obiettivo", "genera i task per", "crea il piano di progetto", "da questo obiettivo tira fuori i task" — or when a project objective needs to become a PIANO.md that Cowork sessions can execute autonomously. Task depth scales with complexity — no fixed limit on task count or nesting.
---

# Pianifica — da obiettivo a sequenza di task

Trasformare un obiettivo di progetto concreto in un file `PIANO.md`: la sequenza completa
di task che le sessioni Cowork eseguiranno, con profondità proporzionale alla complessità.
Il piano è il contratto con l'esecutore — deve funzionare senza il contesto di questa chat.

**Prima di tutto:** leggere `references/formato-piano.md` (dentro la cartella di questa
skill) per il formato esatto del piano e le regole sui task. Il file scritto deve
rispettarlo alla lettera.

L'obiettivo arriva come argomento: $ARGUMENTS

## Procedura

### 1. Decodificare l'obiettivo

Verificare che l'obiettivo contenga, o si possa inferire con certezza:
- il **deliverable finale** (cosa esiste alla fine che ora non esiste);
- la **definizione di finito** (condizione osservabile che chiude il progetto);
- i **vincoli** rilevanti (strumenti, scadenze, budget, decisioni già prese, percorsi).

Se manca qualcosa di decisivo, porre al massimo 3 domande in un solo messaggio, con
opzioni decidibili — non liste aperte. Se l'informazione è inferibile senza rischio,
inferirla e dichiararla nel piano sotto "Vincoli e contesto" come assunzione.

### 2. Valutare la complessità

Stimare la taglia reale del progetto prima di decomporre:
- **Semplice** — un deliverable, nessuna dipendenza esterna: 3-7 task piatti, una fase.
- **Medio** — più deliverable o più competenze coinvolte: 2-4 fasi, task con dipendenze.
- **Complesso** — deliverable multipli, incertezza, iterazioni previste: fasi + task
  composti con sotto-task ricorsivi, senza limite di numero o profondità.

Non gonfiare i progetti semplici per sembrare completi; non appiattire quelli complessi
per sembrare gestibili.

### 3. Decomporre

Procedere dall'alto verso il basso:
1. Individuare le **fasi** (macro-tappe ordinate verso il deliverable finale).
2. Dentro ogni fase, elencare i **task**.
3. Per ogni task applicare il test di atomicità (una sessione Cowork, criterio
   osservabile, autosufficiente — dettagli nel formato). Se fallisce il test,
   decomporlo in sotto-task e ripetere il test sui figli, ricorsivamente.
4. Dichiarare le **dipendenze** tra task con gli ID. Massimizzare i task senza
   dipendenze reciproche: più task eseguibili in parallelo = più sessioni Cowork
   possono lavorare insieme.

Includere sempre, dove ha senso, task di **verifica** (controllare il deliverable contro
il criterio di accettazione) e il task finale di **chiusura** che verifica la definizione
di finito.

### 4. Scrivere PIANO.md

- Posizione: cartella radice del progetto. Se non è ovvia dal contesto, chiedere.
- Se esiste già un `PIANO.md` per lo stesso progetto: NON sovrascriverlo — proporre
  invece la skill orchestratore-ripianifica. Se è un progetto diverso nella stessa
  cartella, usare `PIANO-<slug>.md`.
- Compilare tutte le sezioni del formato, Registro incluso (prima riga: piano creato).
- "Prossimo task" nella sezione Stato = il primo task eseguibile.

### 5. Registrare il piano nella torre di controllo

Aggiungere una riga alla nota `<vault>/99 Sistema/Piani attivi.md`:
`| <nome progetto> | <percorso PIANO.md> | <AAAA-MM-GG> | attivo |`.
Aggiornamento incrementale: solo la riga nuova, mai riscrivere la nota. Se la sessione
non ha accesso al vault (es. Cowork su un'altra cartella), segnalarlo nella sintesi
finale: la riga andrà aggiunta alla prossima scansione di orchestratore-cruscotto.

### 6. Sintesi finale

Riportare all'utente, in breve:
- numero di fasi e task (e profondità massima raggiunta);
- il primo task eseguibile;
- le assunzioni fatte al punto 1, se ce ne sono;
- come proseguire: skill orchestratore-esegui in una sessione Cowork aperta sulla
  cartella del progetto.

## Prossime skill

Al termine del processo, proponi all'utente le conseguenze sceglibili pertinenti
tra queste — solo quelle che gli output di questa sessione rendono sensate:

- se il piano è scritto e approvato → **orchestratore-esegui** (esegue il primo task eseguibile)
- se l'obiettivo cambia prima ancora di iniziare → **orchestratore-ripianifica** (aggiorna il piano senza rifarlo)
- se hai più piani aperti e vuoi la vista d'insieme → **orchestratore-cruscotto** (dove serve l'utente)
- oppure: fermati qui — il piano è salvo, qualsiasi sessione Cowork futura riparte da lì.
