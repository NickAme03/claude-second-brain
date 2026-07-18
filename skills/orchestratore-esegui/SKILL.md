---
name: orchestratore-esegui
description: This skill should be used when the user asks to execute tasks from an existing PIANO.md — trigger phrases such as "esegui", "esegui il piano", "prossimo task", "continua il piano", "vai col piano", "riprendi il progetto", "a che punto siamo col piano" — or when a Cowork session opens on a project folder containing a PIANO.md and work should resume from where the last session stopped.
---

# Esegui — dal piano al lavoro fatto

Eseguire il prossimo task eseguibile di un `PIANO.md`, verificarlo contro il suo criterio
di accettazione e aggiornare lo stato del piano. Il piano è l'unica fonte di verità:
qualsiasi sessione può riprendere da qui senza altro contesto.

**Prima di tutto:** leggere
`~/.claude/skills/orchestratore-pianifica/references/formato-piano.md`
per il formato del piano, i marcatori di stato e le regole di aggiornamento.

Argomento opzionale (percorso del piano, o ID di un task specifico): $ARGUMENTS

## Procedura

### 1. Trovare e leggere il piano

- Se $ARGUMENTS contiene un percorso, usarlo. Altrimenti cercare `PIANO.md` (o
  `PIANO-*.md`) nella cartella di lavoro corrente; con più piani trovati, chiedere quale.
- Leggere la sezione `## Stato` per orientarsi, poi i task.
- Se non esiste alcun piano: proporre la skill orchestratore-pianifica e fermarsi.

### 2. Scegliere il task

- Se $ARGUMENTS indica un ID, usare quello (avvisando se ha dipendenze non completate).
- Altrimenti: il primo task foglia `- [ ]` con tutte le dipendenze `- [x]`, in ordine di
  documento. Un task `- [~]` (in corso, lasciato da una sessione interrotta) ha
  precedenza: riprenderlo.
- Se non c'è nessun task eseguibile ma il piano non è completo → i task rimasti sono
  bloccati o dipendono da task bloccati: riportare la situazione e proporre
  orchestratore-ripianifica.
- Se tutti i task sono completati → verificare la definizione di finito nella sezione
  Obiettivo e dichiarare il progetto chiuso (riga nel Registro).

### 3. Eseguire

1. Marcare il task `- [~]` e salvare (così una sessione interrotta lascia traccia).
2. Eseguire il lavoro descritto, rispettando "Vincoli e contesto".
3. Verificare il risultato contro il criterio di **Accettazione** del task — davvero,
   non dichiarativamente: aprire il file, lanciare il comando, guardare l'output.

### 4. Aggiornare il piano

- Verifica superata → `- [x]`, riga nel Registro (`AAAA-MM-GG | Completato X.Y: <esito in breve>`).
- Verifica fallita e non risolvibile dentro il task → `- [!]` con `- Blocco: <motivo>`,
  riga nel Registro, e proporre orchestratore-ripianifica.
- Un task composto diventa `- [x]` solo quando tutti i figli lo sono.
- Riallineare la sezione `## Stato` (avanzamento, prossimo task, data).

### 5. Riportare e proseguire

Riportare in breve: task eseguito, esito della verifica, prossimo task eseguibile.
Poi proseguire col task successivo, fermandosi solo quando: il piano è completo, un
blocco richiede ripianificazione, oppure il prossimo task richiede una decisione o
un input che solo l'utente può dare (e in quel caso chiedere).

## Prossime skill

Al termine del processo, proponi all'utente le conseguenze sceglibili pertinenti
tra queste — solo quelle che gli output di questa sessione rendono sensate:

- se un task è bloccato o il piano non regge alla realtà → **orchestratore-ripianifica** (aggiornamento incrementale del piano)
- se il piano è completato ma emergono lavori nuovi → **orchestratore-pianifica** (piano per il progetto successivo)
- se il deliverable completato verrà usato per decisioni reali → **verifica-rigore** (revisione avversariale prima di consegnarlo) *(link grigio, se assente nel sistema)*
- oppure: fermati qui — lo stato è salvo nel piano, la prossima sessione riprende da lì.
