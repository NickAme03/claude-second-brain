---
name: skill-linker
description: Manages incremental updates to other skills by turning every process into a chain of choosable consequences — it adds a standard 'Prossime skill' section (2 to 4 condition-to-skill links) to each skill, maintains the link-graph registry, and always updates one skill at a time. Use whenever the user wants to update, link, or network their skills. Gestisce gli aggiornamenti incrementali delle altre skill trasformando ogni processo in una catena di conseguenze sceglibili; aggiunge a ogni skill una sezione standard "Prossime skill" (2-4 collegamenti condizione → skill), mantiene il registro del grafo dei collegamenti e aggiorna sempre una skill per volta. Usa questa skill ogni volta che l'utente vuole aggiornare, collegare o mettere in rete le proprie skill — trigger tipici; "aggiorna le skill", "collega le skill", "aggiungi handoff a", "il grafo delle skill", "quale skill viene dopo questa", "rendi le skill una catena" — e anche quando, a fine lavoro con un'altra skill, emerge che manca un collegamento in uscita che sarebbe stato utile.
---

# Skill Linker

Il sistema di skill dell'utente funziona come il suo vault: **l'argomento lo fanno i link, non le cartelle**. Una skill senza collegamenti in uscita è una skill morta — finisce il suo lavoro e lascia l'utente a ricordarsi da solo il catalogo. Questa skill trasforma, un aggiornamento alla volta, ogni skill in un nodo di un grafo: al termine di ogni processo l'utente sceglie la conseguenza successiva da 2-4 opzioni proposte, invece di dover pensare "e adesso?".

## Principio guida

**Ogni processo termina con conseguenze sceglibili, mai con un vicolo cieco.** Lo scopo non è automatizzare la scelta (la scelta resta all'utente) ma azzerare lo sforzo di ricordare cosa esiste. La proposta giusta arriva dal lavoro appena fatto: sono gli *output reali* della skill a determinare quali skill vengono dopo, non un elenco generico.

## Formato standard della sezione di handoff

Ogni skill aggiornata riceve in coda al suo SKILL.md questa sezione, identica per struttura in tutto il sistema:

```markdown
## Prossime skill

Al termine del processo, proponi all'utente le conseguenze sceglibili pertinenti
tra queste — solo quelle che gli output di questa sessione rendono sensate:

- se [condizione osservabile negli output] → **[nome-skill]** ([cosa farà])
- se [condizione] → **[nome-skill]** ([cosa farà])
- se [condizione] → **[nome-skill]** ([cosa farà])
- oppure: fermati qui — il lavoro è completo così.
```

Regole del formato:
- **2-4 collegamenti**, mai di più: cinque opzioni sono un catalogo, non una scelta.
- La **condizione precede la skill**: l'utente riconosce la propria situazione, non il nome dello strumento.
- L'**ultima riga è sempre l'uscita** ("fermati qui"): una catena senza uscita diventa una trappola che allunga ogni sessione.
- Le condizioni sono **osservabili negli output** ("se il report rivela progetti fermi"), mai generiche ("se vuoi approfondire").

Esempio reale (per report-personale):

```markdown
## Prossime skill

- se il report verrà usato per decisioni reali → **verifica-rigore** (revisione avversariale dei claim)
- se D4 rivela progetti fermi o sproporzione piano/esecuzione → **gestione-progetti** (inquadramento e prossime azioni)
- se emergono lacune di comprensione su un tema del corpus → **apprendimento-adattivo** (colmarle con spiegazioni calibrate)
- oppure: fermati qui — il report è la baseline per il prossimo confronto diacronico.
```

## Workflow di aggiornamento — una skill per volta

Mai aggiornamenti in blocco: ogni skill va capita prima di essere collegata, e un lotto di modifiche superficiali produce collegamenti generici che nessuno sceglierà. Per ogni aggiornamento:

1. **Individua il file reale.** Chiedi all'utente quale skill aggiornare, poi trova il suo SKILL.md. Attenzione al percorso: le skill in directory gestite (percorsi di sessione tipo `AppData\Roaming\Claude\...\skills-plugin\<uuid>\...`) sono copie sincronizzate dall'app — modificarle sul disco è inutile. **Decisione presa (6 lug 2026): la fonte è il gestore skill dell'app.** Il processo è quindi: prepara il testo aggiornato in `~/.claude/skill-updates\<nome-skill>.md` (istruzioni di modifica + sezione nuova, pronte da incollare), l'utente lo applica nell'editor skill dell'app, e solo dopo la sua conferma spunti il registro. Le skill native di Claude Code in `~/.claude/skills/` si modificano invece direttamente sul file.
2. **Leggi la skill per intero.** I collegamenti si derivano dagli output che la skill produce davvero, non dalla sua descrizione.
3. **Mappa gli esiti.** Elenca i 2-4 stati in cui l'utente si trova tipicamente a fine processo (un report consegnato, un piano fatto, un dubbio emerso, un progetto sbloccato).
4. **Deriva le conseguenze.** Per ogni esito, chiedi: quale skill del registro è la mossa naturale da qui? Se una skill esistente ha già una sezione di collegamenti non standard (es. "Interazione con le altre skill"), assorbila nella nuova sezione invece di duplicarla.
5. **Mostra il diff e attendi l'ok.** Solo la sezione "Prossime skill" in coda (ed eventuale rimozione della vecchia sezione assorbita): il corpo della skill non si tocca mai — questa skill aggiunge collegamenti, non riscrive metodi.
6. **Aggiorna il registro** qui sotto nello stesso task: un grafo non tracciato torna a essere un catalogo.

## Registro del grafo

Stato dei collegamenti, da verificare sul file reale alla prima apertura di ogni skill. I collegamenti mancanti sono i "link grigi" del sistema: mappa dei buchi, non errori. Il registro sottostante è un **template con righe d'esempio**: sostituiscilo con le TUE skill alla prima esecuzione, e da lì tienilo vivo — ogni aggiornamento di una skill aggiorna anche la sua riga.

| Skill | Sezione "Prossime skill" | Collegamenti in uscita noti | Note |
|---|---|---|---|
| risparmio-token | ☑ | skill-creator, skill-linker, verifica-rigore | nata con sezione standard |
| decodifica-intento | ☑ | anti-compiacimento, gestione-progetti | pre-processing delle richieste, aggancio permanente via regola in memoria |
| analista-progetti | ☑ | orchestratore-pianifica, percorsi-di-sviluppo, verifica-rigore | review progetti/portfolio con lente employability; onestà ereditata da anti-compiacimento |
| *(le tue skill…)* | ☐ | | |

Legenda: ☐ = da fare · ◪ = testo pronto, in attesa di applicazione nell'editor dell'app · ☑ = sezione standard presente. Quando una skill viene aggiornata, spunta la casella e riporta i collegamenti effettivi nella terza colonna.

## Regole non negoziabili

1. Una skill per volta, sempre con diff mostrato prima della scrittura.
2. Solo la sezione "Prossime skill" (più l'assorbimento di eventuali sezioni di collegamento preesistenti): mai toccare metodo, regole o workflow della skill ospite.
3. Ogni modifica aggiorna il registro nello stesso task, o il grafo smette di essere affidabile.
4. Se un collegamento sensato punta a una skill che non esiste ancora, registralo comunque nel registro come link grigio: è la todo-list naturale del sistema, esattamente come nel vault.

## Prossime skill

- se hai appena aggiornato una skill e il registro mostra altri ☐ → **skill-linker** di nuovo, sulla prossima candidata (una per volta)
- se durante la mappatura è emerso che serve una skill nuova, non un collegamento → **skill-creator** (crearla, poi collegarla)
- se vuoi verificare che i collegamenti scelti reggano all'uso reale → esegui la skill appena aggiornata su un caso vero e osserva se la proposta finale è quella giusta
- oppure: fermati qui — il grafo cresce un nodo alla volta.
