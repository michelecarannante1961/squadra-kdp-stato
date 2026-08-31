# Squadra KDP — Stato condiviso

Questo repository è la "memoria condivisa" dell'Agente Ricerca Keyword (Stadio 1 della pipeline
editoriale KDP di Michele). Più agenti AI diversi — Claude Code (routine cloud schedulata) e
altri agenti agentic come Google Antigravity/Gemini — leggono e scrivono qui, così lo storico
delle keyword già analizzate resta unico indipendentemente da chi esegue la ricerca in un dato
giorno.

Istruzioni operative complete per l'agente: [`PROMPT.md`](./PROMPT.md).

## Struttura

- `seeds.txt` — categorie/argomenti seme da cui partire per la ricerca keyword.
- `storico.csv` — storico di tutte le keyword analizzate (append-only), colonne:
  `data,seed,mercato,keyword,domanda,concorrenza,opportunita,note`.
- `trend_storico.csv` — storico delle verifiche di trend (Stadio 2 della pipeline).
- `report_<YYYY-MM-DD>.md` — report giornaliero della ricerca keyword.
- `trend_report_<YYYY-MM-DD>.md` — report giornaliero di validazione trend.
- `soffitta/` — storyboard/idee valutate e accantonate (non cancellate, solo archiviate).

Questo repository copre solo lo **Stadio 1 (Ricerca Keyword)** e, quando presente,
**Stadio 2 (Validazione Trend)** della pipeline completa a 7 stadi. Gli stadi successivi
(storyboard, scrittura, landing page) restano gestiti localmente da Michele con Claude Code, dato
che coinvolgono decisioni editoriali che richiedono i suoi checkpoint espliciti.
