---
name: orchestratore-cruscotto
description: This skill should be used when the user wants a single view over all active PIANO.md plans — trigger phrases such as "cruscotto", "a che punto sono i progetti", "dove serve l'utente", "stato dei piani", "panoramica dei piani", "quali progetti sono bloccati", "torre di controllo" — or as part of an end-of-day report when multiple orchestrated projects are open. Read-only on the plans; answers one question — where is the user's attention actually needed.
---

# Cruscotto — la torre di controllo dei piani

Dare una vista unica su tutti i piani attivi e rispondere a una sola domanda: **dove
serve l'utente?** I progetti che avanzano da soli non hanno bisogno di attenzione; quelli
bloccati sì. Il cruscotto legge, non tocca mai i PIANO.md.

**Prima di tutto:** leggere
`~/.claude/skills/orchestratore-pianifica/references/formato-piano.md`
per i marcatori di stato dei task.

## Fonti

- **Registro dei piani**: `<vault>/99 Sistema/Piani attivi.md`
  — una riga per piano (progetto, percorso, data, stato). Se la nota non esiste o è
  vuota: nessun piano registrato, proporre orchestratore-pianifica e fermarsi.
- **I singoli PIANO.md** ai percorsi elencati con stato `attivo`.

## Procedura

### 1. Scansione (leggera)

Per ogni riga `attivo` del registro, leggere del PIANO.md SOLO ciò che serve:
la sezione `## Stato` e le righe dei task `- [!]` con i loro `- Blocco:` (usare Grep,
non leggere i piani per intero). Se il percorso non esiste, annotarlo.

### 2. Sincronizzare il registro

Aggiornare in modo incrementale le righe di `Piani attivi.md`:
- piano con tutti i task chiusi e definizione di finito verificata a registro → stato `completato`;
- percorso non trovato → stato `irreperibile` (mai cancellare la riga);
- il resto non si tocca. Mai modificare i PIANO.md da qui.

### 3. Riportare — ordinato per attenzione richiesta

Report compatto in tre blocchi, in quest'ordine:

1. **Serve l'utente** — piani con task `- [!]`: progetto, task bloccato, motivo del
   blocco, e la decisione concreta da prendere (una riga ciascuno). Se il blocco è
   risolvibile con una ripianificazione già chiara, dirlo.
2. **Avanzano da soli** — piani con task eseguibili: progetto, avanzamento n/totale,
   prossimo task. Nessuna azione richiesta: basta una sessione Cowork con la skill
   orchestratore-esegui.
3. **Chiusi dall'ultima scansione** — completati, chiusi, irreperibili.

Chiudere con una riga di raccomandazione: la singola azione a maggior leva adesso
(sbloccare X, oppure spingere Y che è al 90%). Se i blocchi sono zero, dirlo
esplicitamente: "oggi il sistema non ha bisogno di te".

## Prossime skill

Al termine del processo, proponi all'utente le conseguenze sceglibili pertinenti
tra queste — solo quelle che gli output di questa sessione rendono sensate:

- se il cruscotto mostra task bloccati → **orchestratore-ripianifica** (sul piano bloccato, uno per volta)
- se un piano avanza da solo e vuoi spingerlo ora → **orchestratore-esegui** (in una sessione sulla cartella di quel progetto)
- se è fine giornata → aggancia questo report al feedback di fine giornata (regola permanente)
- oppure: fermati qui — la panoramica è sincronizzata.
