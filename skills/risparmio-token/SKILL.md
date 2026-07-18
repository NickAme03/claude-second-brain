---
name: risparmio-token
description: Reduces token consumption in Claude sessions (Claude Code, Cowork, API) using Anthropic's documented techniques — context hygiene, cache-friendliness, subagent delegation, essential output, progressive disclosure — across three modes (operating rules, setup audit, token-efficient design). Use whenever the user talks about consumption, cost, or limits, or when designing a new skill or CLAUDE.md. Riduce i token consumati nelle sessioni Claude (Claude Code, Cowork, API) applicando le tecniche documentate da Anthropic — igiene del contesto, cache-friendliness, delega ai subagenti, output essenziale, progressive disclosure — in tre modalità; regole operative durante il lavoro, audit del setup (CLAUDE.md, MCP, skill), progettazione di skill e prompt token-efficienti. Usa questa skill ogni volta che l'utente parla di consumo, costi o limiti — trigger tipici; "sto finendo i token", "consuma troppo", "ottimizza i token", "riduci il contesto", "perché ho bruciato il limite", "sessione troppo pesante", "audit del mio setup" — e quando progetti una skill o un CLAUDE.md nuovi, anche se l'utente non nomina i token.
---

# Risparmio Token

I token si consumano in tre punti: contesto in ingresso (riletto a OGNI turno), output generato (thinking incluso), e cache mancata. Ogni tecnica qui sotto agisce su uno di questi. Questa skill è essa stessa a dieta: solo ciò che serve a decidere, niente teoria.

## Fatti di base (da cui discende tutto)

- Il contesto è un **costo ricorrente**: ogni cosa nel contesto viene ripagata a ogni messaggio. 1.000 token inutili × 50 turni = 50.000 token sprecati.
- La **cache è un prefisso**: scrittura +25%, lettura 10% del prezzo base, TTL 5 minuti che si rinnova a ogni lettura. Un solo token cambiato nel prefisso = cache persa. Stabilità del prefisso > tutto.
- Il **thinking è fatturato come output** e può valere decine di migliaia di token a richiesta.
- L'**output di oggi è l'input di domani**: la verbosità si paga due volte.

## Modalità 1 — Regole operative (durante la sessione)

Applica queste regole mentre lavori; valgono per Claude, non per l'utente:

1. **Grep prima di Read, Read con offset/limit prima di Read intero.** Leggi la porzione che serve, non il file. Un log da 10.000 righe si filtra (`grep ERROR`), non si legge.
2. **Mai rileggere ciò che è già in contesto.** Niente riletture di verifica dopo un Edit riuscito.
3. **Chiamate indipendenti in parallelo, nello stesso blocco.** Ogni round-trip evitato è contesto che non cresce.
4. **Delega il verboso ai subagenti**: test, fetch di documentazione, esplorazioni larghe. L'output resta nel loro contesto, a te torna il sommario.
5. **Output essenziale**: rispondi alla domanda, non al suo intorno. Formato vincolato (lista, tabella corta) quando la prosa inviterebbe verbosità.
6. **Prompt di ritorno specifici**: quando proponi il passo successivo all'utente, formularlo già scoped (file, funzione, criterio di fatto) — un prompt vago innesca scansioni larghe al turno dopo.

## Modalità 2 — Audit del setup

Quando l'utente chiede perché consuma tanto, esamina nell'ordine (impatto decrescente):

1. **CLAUDE.md**: è sotto le ~200 righe? Contiene istruzioni per workflow specifici che potrebbero diventare skill (caricate on-demand invece che a ogni sessione)?
2. **Server MCP**: quanti sono configurati e quanti usati davvero? Ogni server attivo pesa; per GitHub/AWS/simili la CLI (`gh`, `aws`) costa meno dei tool MCP. Suggerisci `/mcp` per disattivare gli inutilizzati e `/context` per vedere cosa occupa spazio.
3. **Abitudini di sessione**: sessioni infinite multi-argomento? Suggerisci `/clear` tra task non correlati (con `/rename` prima per ritrovarla), `/compact` con istruzioni mirate a fine fase (non a degrado avvenuto), plan mode per i task complessi (l'errore di direzione è la voce di spreco più grande).
4. **Thinking ed effort**: per task semplici suggerisci `/effort` più basso; il default può essere sovradimensionato.
5. **Modello**: Sonnet per il lavoro ordinario, Opus/Fable solo dove serve ragionamento profondo; `haiku` per i subagenti semplici.
6. **Strumenti di misura prima di tutto**: `/usage` (con attribuzione per skill/subagenti/MCP) e `/context` — mai diagnosi a occhio quando esiste il dato.

## Modalità 3 — Progettare artefatti token-efficienti

Quando si scrive una skill, un CLAUDE.md o un prompt riusabile:

- **Progressive disclosure**: metadata sempre in contesto (~100 token), corpo caricato al trigger (<5.000 token), riferimenti in file separati letti solo quando servono. Se due contenuti non servono mai insieme, separali in file diversi.
- **Script invece di generazione**: un lavoro deterministico (ordinare, estrarre, convertire) va in uno script eseguibile bundled — l'esecuzione non carica né lo script né i dati in contesto. Generare token per fare ciò che fa un algoritmo è lo spreco peggiore.
- **Cache-friendliness**: contenuto stabile all'inizio (system, istruzioni), contenuto variabile alla fine. Non toccare il prefisso a metà sessione.
- **Il preprocessing sta nei hook**: se un output va sempre filtrato (test, log), il filtro va in un hook PreToolUse, non nella pazienza del modello.

## Fonti

Basata su: [Prompt caching (docs ufficiali)](https://platform.claude.com/docs/en/build-with-claude/prompt-caching) · [Manage costs effectively (Claude Code docs)](https://code.claude.com/docs/en/costs) · [Agent Skills (Anthropic engineering)](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills) · [Effective context engineering (Anthropic engineering)](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) · [Context management (Anthropic)](https://www.anthropic.com/news/context-management). Verificata luglio 2026; prezzi e comandi possono cambiare — in caso di dubbio, rileggere le docs.

## Prossime skill

- se l'audit rivela istruzioni da spostare da CLAUDE.md a una skill nuova → **skill-creator** (crearla con progressive disclosure già impostata) *(link grigio, se assente nel sistema)*
- se la skill nuova esiste e va messa in rete → **skill-linker** (collegarla al grafo)
- se le raccomandazioni dell'audit verranno usate per decisioni con costi reali → **verifica-rigore** (controllo avversariale dei numeri prima di agire) *(link grigio, se assente nel sistema)*
- oppure: fermati qui — le regole operative restano attive per la sessione.
