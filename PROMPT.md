# PROMPT UNIVERSALE — Agente Ricerca Keyword KDP (stato condiviso su GitHub)

Questo prompt è pensato per essere eseguito **da qualunque agente AI capace di usare un
terminale/git e di cercare sul web** (Claude Code — sia in una sessione interattiva sia come
routine cloud schedulata — Google Antigravity/Gemini, o altri agenti agentic simili). Più agenti
diversi possono eseguirlo sullo stesso repository, in giorni diversi o anche nello stesso giorno,
senza perdere lo storico: il repository Git è la "memoria condivisa" tra tutti loro.

**Repository di stato**: `https://github.com/michelecarannante1961/squadra-kdp-stato`

---

## Cosa devi fare, in ordine

### 0. Prerequisiti d'ambiente

- Devi avere accesso a un terminale con `git` disponibile e le credenziali per clonare/pushare
  sul repository indicato sopra (se il repo è privato, l'operatore umano deve averti già dato le
  credenziali/token necessari prima di eseguire questo prompt — se non le hai, fermati e chiedile).
- Devi avere accesso a uno strumento di ricerca web (web search o browsing).
- Se il repository non è ancora stato clonato in locale, clonalo in una cartella di lavoro
  temporanea. Se hai già un checkout locale aggiornalo con `git pull` prima di iniziare.

### 1. Sincronizza lo stato

```
git pull origin main
```

Se il pull fallisce per conflitti (un altro agente ha scritto nel frattempo), non forzare: risolvi
il conflitto mantenendo entrambe le righe/sezioni in conflitto (lo storico è fatto per crescere per
righe append-only, quasi mai per essere sovrascritto — in caso di dubbio tieni entrambe le versioni
e segnala il conflitto nel report finale).

### 2. Leggi lo stato esistente nel repository

- `seeds.txt` — categorie/argomenti seme, una per riga. Se il file non esiste ancora nel
  repository, crealo con questi seed di partenza: crescita personale, ricette e cucina, attività
  per bambini, fitness e dimagrimento, finanza personale, hobby e manualità, rimedi naturali e
  salute, planner e agende, narrativa romance, narrativa fantasy, meditazione e spiritualità,
  viaggi, animali domestici, scuola e compiti, matrimoni ed eventi.
- `storico.csv` — colonne: `data,seed,mercato,keyword,domanda,concorrenza,opportunita,note`.
  `mercato` vale `IT` o `EN`. Se non esiste, crealo con solo l'header.
- `report_<YYYY-MM-DD>.md` — un file per ogni giorno in cui l'agente (qualunque esso sia) ha
  eseguito la ricerca. **Se esiste già un report per la data odierna** (perché un altro agente ha
  già girato oggi), non sovrascriverlo: aggiungi in append una sezione `## Sessione N — eseguita
  da <nome del tuo sistema/modello>` con i tuoi risultati aggiuntivi, scegliendo seed diversi da
  quelli già coperti oggi.

### 3. Obiettivo della ricerca

Dare, ogni volta che questo prompt viene eseguito, una piccola lista di keyword/nicchie
potenzialmente profittevoli per il self-publishing su Amazon KDP, **confrontando mercato italiano
(Amazon.it) e mercato internazionale in inglese (Amazon.com)**, usando **solo fonti pubbliche
gratuite** (ricerche web, pagine Amazon pubbliche, suggerimenti di ricerca) — mai dati a pagamento
o scraping massivo.

Il valore aggiunto del confronto tra mercati: una nicchia può essere satura in italiano ma libera
in inglese (o viceversa), oppure profittevole in entrambi — è il segnale più utile da evidenziare.

**Limite onesto da dichiarare sempre nel report**: non hai un database proprietario di vendite
storiche come gli strumenti a pagamento (BookBeam, Publisher Rocket). Fornisci segnali indicativi
(interesse di ricerca, livello di concorrenza visibile), non dati di vendita reali. Dichiaralo
sempre, in una riga, senza essere pedante.

### 4. Procedura

1. Scegli 2-3 seed per questa sessione, priorità a quelli meno analizzati di recente (guarda le
   date in `storico.csv`). Ruota i seed rispetto alle sessioni precedenti — non ripetere sempre le
   stesse categorie, e non riproporre una keyword già vista nello stesso mercato negli ultimi 20
   giorni a meno che nuovi segnali cambino la valutazione.
2. Per ogni seed, genera 2-3 keyword candidate PER MERCATO (IT e EN):
   - **IT**: cerca sul web query tipo "amazon.it libri \<seed\> idee popolari", "cosa cercano le
     persone quando cercano libri di \<seed\>".
   - **EN**: traduci il seed in inglese e cerca "amazon.com kindle books \<seed EN\> popular
     ideas", "best selling \<seed EN\> kdp niche".
   - Preferisci keyword long-tail (2-4 parole). Cerca, quando possibile, la stessa idea di nicchia
     espressa nei due mercati per poterla confrontare direttamente.
3. Per ogni keyword, valuta:
   - **Domanda** (Alta/Media/Bassa): quante fonti diverse la suggeriscono, se compare in
     articoli/blog recenti, se ha varianti multiple.
   - **Concorrenza** (Alta/Media/Bassa): cerca `site:amazon.it <keyword>` o `site:amazon.com
     <keyword>` e valuta numero di libri dedicati, quanto sono recenti, quante recensioni hanno i
     primi risultati.
   - **Opportunità**: 🟢 Alta (domanda Alta/Media + concorrenza Bassa/Media) / 🟡 Da valutare
     (domanda Media + concorrenza Media, o segnali contrastanti) / 🔴 Bassa (domanda Bassa, o
     concorrenza Alta con player consolidati).
4. Scrivi/aggiorna il report del giorno con: seed analizzati, le 2-3 migliori opportunità in cima,
   un blocco "confronto mercati" (es. "🇮🇹 saturo / 🌍 spazio libero"), la tabella completa, e il
   disclaimer sul limite dei dati.
5. Aggiorna `storico.csv` in append con le righe di oggi.
6. **Commit e push**:
   ```
   git add seeds.txt storico.csv report_<data>.md
   git commit -m "Ricerca keyword KDP <data> — eseguita da <nome del tuo sistema>"
   git push origin main
   ```
   Se il push viene rifiutato perché il remoto è avanzato (un altro agente ha pushato nel
   frattempo), fai `git pull --rebase origin main` e ripeti il push. Non forzare mai
   (`--force`) un push su questo repository.
7. **Comunica il risultato all'operatore umano** nel modo che il tuo sistema prevede (messaggio in
   chat, notifica, file inviato) con un riassunto breve: le 2-3 migliori opportunità trovate oggi,
   con motivazione.

### Regole di prudenza

- Non più di ~15-18 richieste web per sessione: è una ricerca leggera quotidiana, non scraping
  massivo.
- Tratta i punteggi come indicativi. Invita sempre l'utente a verificare manualmente su Amazon le
  keyword 🟢 prima di scrivere un libro su quel tema.
- Non promettere mai risultati di vendita.
- Non pushare mai forzatamente (`git push --force`) su questo repository: se c'è un conflitto,
  fermati e segnalalo invece di sovrascrivere il lavoro di un altro agente.

---

## Come collegare più agenti allo stesso repository

- **Claude Code (routine cloud schedulata)**: la routine deve avere il repository sopra tra i
  `sources` della sessione, e il prompt della routine è il contenuto di questo file (dalla sezione
  "Cosa devi fare" in giù), con l'URL del repository già inserito.
- **Google Antigravity / Gemini o altro agente agentic**: incolla lo stesso contenuto (dalla
  sezione "Cosa devi fare" in giù) come istruzione iniziale, assicurandoti che l'agente abbia
  accesso a un terminale con `git` configurato per quel repository e a uno strumento di ricerca
  web.
- Ogni agente firma il proprio lavoro nel commit e nella sezione del report ("eseguita da
  Claude"/"eseguita da Gemini/Antigravity"/ecc.) così è sempre chiaro chi ha prodotto cosa, e più
  esecuzioni nello stesso giorno da sistemi diversi si sommano invece di sovrascriversi.
