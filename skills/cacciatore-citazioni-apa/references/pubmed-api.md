# PubMed E-utilities — guida operativa

API pubblica e gratuita del NCBI. Nessuna chiave necessaria per un uso leggero (max ~3 richieste/secondo). Base URL:
`https://eutils.ncbi.nlm.nih.gov/entrez/eutils/`

Interrogala con `WebFetch`, passando come `prompt` cosa vuoi estrarre dal risultato.

## Passo 1 — esearch: dai termini ai PMID

```
https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi?db=pubmed&retmode=json&retmax=10&sort=relevance&term=<QUERY>
```

- `<QUERY>`: termini in inglese, con operatori booleani e tag di campo. Spazi → `+`, virgolette → `%22`.
- Esempi di query efficaci:
  - `anosognosia AND (Alzheimer disease) AND awareness`
  - `facial emotion recognition AND Alzheimer disease`
  - `metamemory AND Alzheimer`
  - Filtri utili: aggiungi `AND review[pt]` per le review, `AND meta-analysis[pt]` per le meta-analisi, `AND 2015:2025[dp]` per l'intervallo di date.
- Restituisce `esearchresult.idlist`: la lista dei PMID.

## Passo 2 — efetch/esummary: dai PMID ai metadati

Per i metadati strutturati (autori, titolo, rivista, anno, volume, pagine, DOI):

```
https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esummary.fcgi?db=pubmed&retmode=json&id=<PMID1>,<PMID2>
```

Per l'**abstract** completo (da cui estrarre la frase verbatim):

```
https://eutils.ncbi.nlm.nih.gov/entrez/eutils/efetch.fcgi?db=pubmed&rettype=abstract&retmode=text&id=<PMID>
```

Campi da estrarre da esummary (JSON, dentro `result.<PMID>`):
- `authors[].name` → cognomi + iniziali
- `title` → titolo articolo
- `fulljournalname` o `source` → **rivista**
- `pubdate` → anno
- `volume`, `issue`, `pages`
- `elocationid` o `articleids[]` con `idtype: "doi"` → **DOI**

## Verifica del DOI

Il DOI risolvibile è `https://doi.org/<DOI>`. Se serve conferma, un `WebFetch` su quell'URL deve puntare all'articolo reale sulla rivista dichiarata. Se il DOI non risolve o punta altrove, la citazione va marcata `[DA VERIFICARE]`.

## Controllo ritrattazioni (obbligatorio nel passo 4 della skill)

Un articolo ritrattato conserva PMID e DOI validi: lo stato va controllato apposta, non emerge dai metadati di base.

- **esummary** (JSON): dentro `result.<PMID>`, il campo `pubtype` — se contiene "Retracted Publication", la fonte è ritrattata: scartala o marcala `[RITRATTATA]`.
- **efetch** (XML, `retmode=xml`): `<PublicationType>` e la lista `<CommentsCorrectionsList>` — `RefType="RetractionIn"` = ritrattato; `RefType="ExpressionOfConcernIn"` = sotto avviso di dubbio; un `ErratumIn` va valutato se tocca proprio il dato citato.
- **Fallback**: la pagina pubblica `https://pubmed.ncbi.nlm.nih.gov/<PMID>/` mostra la ritrattazione in un banner ben visibile a inizio pagina.

## Nota sulla frase verbatim

La frase verbatim va copiata dall'abstract restituito da `efetch` (o dal full text se accessibile), **parola per parola**, indicando la posizione (es. "Abstract, para. 1"). Non ricostruire la frase a memoria: aprila e copiala.

## Fallback

Se l'API non risponde, la pagina pubblica del record è `https://pubmed.ncbi.nlm.nih.gov/<PMID>/` e si può leggere con `WebFetch`. Rimane la fonte di verità per titolo, autori, rivista e abstract.
