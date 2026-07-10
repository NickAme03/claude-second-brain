# Formato PIANO.md — contratto tra pianificatore ed esecutore

Questo file definisce il formato del piano. Tutte le skill dell'orchestratore
(orchestratore-pianifica, orchestratore-esegui, orchestratore-ripianifica,
orchestratore-cruscotto) leggono e scrivono PIANO.md rispettando ESATTAMENTE questa
struttura: è il contratto che permette a qualsiasi sessione Cowork futura di riprendere
il lavoro senza il contesto della chat originale.

## Struttura completa

```markdown
# Piano — <nome progetto>

## Obiettivo
<Obiettivo finale concreto in 1-3 frasi.>

**Definizione di finito:** <condizione osservabile che chiude il progetto — quando questa
è vera, il progetto è completo. Mai vaga ("il sito è bello") ma verificabile ("il sito è
online su dominio X, le 5 pagine caricano, il form invia email").>

## Stato
- Avanzamento: <n>/<totale> task completati
- Prossimo task: <ID del prossimo task eseguibile, oppure "— (piano completato)" o "— (bloccato: vedi X.Y)">
- Ultimo aggiornamento: <AAAA-MM-GG>

## Vincoli e contesto
- <vincolo o informazione necessaria all'esecutore: strumenti, scadenze, budget, decisioni già prese, percorsi file>
- <...>

## Task

### Fase 1 — <nome fase>
- [ ] **1.1** <titolo del task>
  - Deliverable: <output concreto prodotto dal task>
  - Accettazione: <criterio osservabile per dire "fatto bene">
  - Dipende da: <ID | —>
- [ ] **1.2** <task composto (solo intestazione, senza Deliverable): si completa quando tutti i figli sono completati>
  - [ ] **1.2.1** <sotto-task>
    - Deliverable: <...>
    - Accettazione: <...>
    - Dipende da: 1.1

### Fase 2 — <nome fase>
- [ ] **2.1** ...

## Registro
| Data | Evento |
|------|--------|
| AAAA-MM-GG | Piano creato: N task in M fasi |
```

## Regole sui task

**Ogni task foglia deve essere:**
1. **Atomico** — eseguibile in una singola sessione Cowork senza interruzioni.
2. **Verificabile** — il criterio di accettazione è osservabile, non un giudizio.
3. **Autosufficiente** — la descrizione basta a eseguirlo senza leggere la chat di origine.
   Se servono file, percorsi o decisioni prese altrove, riportarli nel task o in
   "Vincoli e contesto".

**Profondità proporzionale alla complessità.** Nessun limite al numero di task o ai
livelli di annidamento. Un task che non è atomico va decomposto in sotto-task
(1.2 → 1.2.1, 1.2.2, ...) ricorsivamente, finché ogni foglia è atomica. Un obiettivo
semplice può produrre 3 task piatti; uno complesso può produrre 40 task su 3 livelli.
Non gonfiare gli obiettivi semplici, non appiattire quelli complessi.

**Dipendenze.** `Dipende da:` elenca gli ID che devono essere completati prima. Un task
senza dipendenze (o con dipendenze tutte completate) è *eseguibile*. Le fasi ordinano la
lettura ma non vincolano: contano solo le dipendenze dichiarate.

## Stati dei task (marcatore checkbox)

| Marcatore | Stato |
|-----------|-------|
| `- [ ]` | da fare |
| `- [~]` | in corso |
| `- [x]` | completato |
| `- [!]` | bloccato — aggiungere sotto il task una riga `- Blocco: <motivo>` |
| `- [-]` | annullato — aggiungere sotto il task una riga `- Annullato: <motivo>` (mai cancellare task dal file) |

## Regole di aggiornamento

- Il file si aggiorna SEMPRE in modo incrementale: mai riscriverlo da zero.
- I task completati non si toccano mai (né testo né stato).
- Ogni modifica strutturale (task aggiunti, annullati, riformulati) e ogni task
  completato o bloccato aggiunge una riga al Registro con data e motivo.
- La sezione `## Stato` si riallinea a ogni modifica: è la cache che l'esecutore legge
  per prima.
- Nome file: `PIANO.md` nella cartella radice del progetto. Se nella stessa cartella
  convivono più progetti, usare `PIANO-<slug>.md`.
- Ogni piano è registrato nella torre di controllo del vault
  (`OBSIDIAN MAIN\Secondo Cervello\99 Sistema\Piani attivi.md`): la riga la scrive
  orchestratore-pianifica alla creazione e la sincronizza orchestratore-cruscotto alla
  scansione. Le altre skill non toccano il registro.
