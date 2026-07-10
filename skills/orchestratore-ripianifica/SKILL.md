---
name: orchestratore-ripianifica
description: This skill should be used when an existing PIANO.md must change — trigger phrases such as "ripianifica", "aggiorna il piano", "il piano non funziona più", "l'obiettivo è cambiato", "questo task è bloccato", "aggiungi questi task al piano", "togli questo task" — or when the orchestratore-esegui skill reports a blocked task or a plan inconsistent with reality. Updates the plan incrementally; never rewrites it from scratch.
---

# Ripianifica — aggiornamento incrementale del piano

Modificare un `PIANO.md` esistente quando la realtà è cambiata: task bloccati, obiettivo
mutato, informazioni nuove, task mancanti o superflui. Il piano si aggiorna con un
registro, non si riscrive mai da zero — la storia delle decisioni resta leggibile.

**Prima di tutto:** leggere
`~/.claude/skills/orchestratore-pianifica/references/formato-piano.md`
per il formato, i marcatori di stato e le regole di aggiornamento.

Argomento opzionale (percorso del piano e/o motivo della ripianificazione): $ARGUMENTS

## Procedura

### 1. Capire cosa è cambiato

Leggere il piano (Stato, Registro, task `- [!]` con i loro `- Blocco:`). Classificare il
motivo della ripianificazione:
- **Blocco locale** — un task è fallito o impraticabile; l'obiettivo resta valido.
- **Obiettivo cambiato** — deliverable finale o definizione di finito sono mutati.
- **Informazione nuova** — vincoli, strumenti o decisioni che invalidano parte dei task.
- **Copertura** — mancano task per arrivare al finito, o alcuni sono diventati inutili.

Se il motivo non è chiaro né da $ARGUMENTS né dal piano, chiedere — una domanda con
opzioni decidibili, non un questionario.

### 2. Delimitare l'impatto

Individuare il sottoinsieme minimo di task toccati dal cambiamento, seguendo le
dipendenze in avanti (un task che dipende da un task annullato va rivisto anche lui).
Tutto il resto NON si tocca. In particolare:
- i task `- [x]` sono intoccabili: né testo, né stato, anche se col senno di poi
  andavano fatti diversamente — l'eventuale rimedio è un task nuovo;
- i task da eliminare diventano `- [-]` con `- Annullato: <motivo>`, mai cancellati.

### 3. Applicare le modifiche

- **Sbloccare**: riformulare il task bloccato o sostituirlo con un percorso alternativo
  (task nuovi che aggirano il blocco); il task originale resta `- [!]` o passa `- [-]`.
- **Aggiungere**: nuovi task con ID coerenti con la fase (nuovi numeri, mai riusare ID
  di task annullati), ognuno con Deliverable, Accettazione, Dipende da. Applicare il
  test di atomicità: decomporre in sotto-task ciò che non è atomico.
- **Obiettivo cambiato**: aggiornare le sezioni Obiettivo e "Vincoli e contesto", poi
  propagare ai task come sopra.
- Riallineare le dipendenze dei task a valle e la sezione `## Stato`.

### 4. Registrare

Aggiungere al Registro una riga per la ripianificazione:
`AAAA-MM-GG | Ripianificato: <motivo> — <n> task aggiunti, <m> annullati, <k> modificati`.
Cambiamenti d'obiettivo meritano una riga propria.

### 5. Sintesi finale

Riportare in breve: motivo, cosa è cambiato (aggiunti / annullati / riformulati con gli
ID), cosa è rimasto intatto, e il nuovo prossimo task eseguibile. Poi proporre
orchestratore-esegui per riprendere.

## Prossime skill

Al termine del processo, proponi all'utente le conseguenze sceglibili pertinenti
tra queste — solo quelle che gli output di questa sessione rendono sensate:

- se il piano è riallineato → **orchestratore-esegui** (riprende dal nuovo prossimo task)
- se oltre metà dei task è stata annullata → **orchestratore-pianifica** (chiudere questo piano con riga finale nel Registro e aprirne uno nuovo, da decidere con l'utente)
- oppure: fermati qui — la modifica è registrata, l'esecuzione può aspettare.
