---
name: cacciatore-citazioni-apa
description: Finds REAL, verifiable academic citations from PubMed (E-utilities) and Google Scholar, formats them in APA 7th edition, extracts the author's verbatim quote, and anchors it to the exact point of an argument, with an absolute ban on fabrication. Use whenever a text needs bibliographic sources, citation checking, or APA formatting. Trova citazioni accademiche REALI e verificabili da PubMed (API E-utilities) e Google Scholar, le formatta in APA 7ª edizione, estrae la frase verbatim dell'autore e la aggancia al punto esatto di un'argomentazione (tesi, paper, capitolo), con divieto assoluto di fabbricazione. Usa questa skill ogni volta che l'utente deve corredare un testo di fonti bibliografiche, cercare citazioni per una tesi o un articolo, verificare che una fonte esista davvero, trasformare affermazioni in claim citati, o formattare riferimenti in APA — trigger tipici: "trova citazioni", "servono le fonti", "cita questa affermazione", "bibliografia APA", "citazioni per il capitolo", "riviste da PubMed/Scholar", "verifica questa citazione", anche se non nomina esplicitamente APA o PubMed. Il valore della skill è produrre SOLO citazioni verificate: mai autori, anni, titoli o frasi inventati.
---

# Cacciatore di citazioni APA

Questa skill serve a corredare un testo argomentativo (una tesi, un capitolo, un paper) di citazioni accademiche **reali, verificabili e formattate in APA 7ª edizione**, dove ogni citazione porta con sé la **frase verbatim** dell'autore e il **punto preciso dell'argomentazione** che sostiene.

Il motivo per cui questa skill esiste è che il rischio numero uno nella ricerca bibliografica assistita da IA è la **fabbricazione**: autori plausibili, anni verosimili, DOI inventati, frasi mai scritte da nessuno. Una tesi difesa a un orale crolla se anche una sola citazione non esiste. Perciò il principio guida non è "trova tante fonti" ma "non riportare nulla che tu non abbia verificato esistere".

## Principio supremo: fabbricazione zero

Questo viene prima di ogni altra cosa e non ha eccezioni:

- **Riporta una citazione SOLO dopo aver recuperato un record reale** (via PubMed E-utilities o pagina reale) che la contiene. Se non riesci a verificarla, scrivi `[DA VERIFICARE]` e spiega cosa manca — non riempire mai il vuoto con un'ipotesi.
- **Non inventare** autori, anno, titolo, rivista, volume, pagine, DOI o PMID. Se un campo non è nel record, lascialo mancante e segnalalo, non dedurlo.
- **Le frasi verbatim si copiano identiche** dalla fonte recuperata (abstract o testo integrale), con la posizione (es. "Abstract, para. 1" o "p. 123"). Una parafrasi non è mai una citazione diretta: se stai parafrasando, dichiaralo, non mettere le virgolette.
- Nel dubbio tra riportare qualcosa di incerto e non riportarlo, **non riportarlo**. Una citazione in meno è un problema minore; una citazione falsa è fatale.

## Flusso di lavoro

### 1. Mappa i claim
Leggi il testo da corredare ed estrai le affermazioni che *richiedono* una fonte: tesi empiriche, dati, definizioni tecniche, posizioni di autori. Per ciascuna, formula in una riga il claim così come compare nel testo. Le affermazioni generiche o di raccordo non hanno bisogno di citazione: non forzarle.

### 2. Cerca su PubMed (fonte primaria, verificabile)
PubMed ha un'API pubblica e gratuita che restituisce record reali con PMID, DOI e rivista. È la spina dorsale della verifica. Il procedimento completo (esearch → efetch, costruzione delle query, parsing) è in [references/pubmed-api.md](references/pubmed-api.md). Interroga con `WebFetch`.

Costruisci query mirate per claim, combinando i termini clinici in inglese (es. `anosognosia AND Alzheimer disease AND awareness`). Preferisci review recenti e meta-analisi per i claim di sfondo, studi primari per i dati specifici.

### 3. Copri Google Scholar (fonte complementare)
Google Scholar non ha API pubblica e blocca lo scraping: usalo via `WebSearch` per allargare la copertura oltre l'ambito biomedico (psicologia, filosofia, scienze cognitive) e per confermare rivista/anno/DOI di un record. Non è una pipeline automatica: ogni risultato va comunque verificato aprendo la fonte.

### 4. Verifica e ancora
Per ogni candidata:
- Apri il record e conferma che esista davvero (PMID/DOI attivo, rivista reale).
- Copia la **frase verbatim** dell'autore dall'abstract/testo, con la posizione.
- Verifica che la frase **sostenga davvero** il claim: non deve essere forzata né estrapolata fuori contesto. Se sostiene solo parzialmente, dillo.

### 5. Formatta in APA 7
Applica lo stile APA 7ª edizione (dettagli e casi particolari in [references/apa7-format.md](references/apa7-format.md)): riferimento completo in bibliografia + citazione in-text `(Autore, anno)`. Riporta sempre la **rivista** e il **database di provenienza** (PubMed / Google Scholar), come parte della tracciabilità.

### 6. Verifica avversariale finale
Prima di consegnare, rileggi ogni citazione chiedendoti: *"Se la relatrice cercasse questo DOI, lo troverebbe? Se aprisse il paper, troverebbe questa frase?"* Scarta tutto ciò che non supera questa prova.

## Formato di output

Una tabella, una riga per citazione:

| Claim del testo | Riferimento APA 7 | Frase verbatim (posizione) | PMID/DOI | Database |

Sotto la tabella, elenca separatamente:
- Le citazioni marcate `[DA VERIFICARE]` con la ragione.
- La bibliografia finale in APA 7, in ordine alfabetico, pronta da incollare.

## Condizioni di stop

- Prima consegna la **tabella delle citazioni verificate per approvazione**; non redigere prosa del testo né modificare file `.docx` prima dell'ok dell'utente.
- Non riscrivere le parti esistenti del testo: la skill lavora sull'apparato citazionale, non sul contenuto altrui.
