# Delegate to Codex

*[English version](README.md)*

## Indice

- [Che Cos'è](#che-cosè)
- [Requisiti](#requisiti)
- [Installazione](#installazione)
  - [Opzione 1: Installazione Dal Marketplace Claude Code](#opzione-1-installazione-dal-marketplace-claude-code)
  - [Opzione 2: Installazione Manuale](#opzione-2-installazione-manuale)
- [Come Usarla Nei Prompt](#come-usarla-nei-prompt)
- [Cosa Insegna La Skill A Claude](#cosa-insegna-la-skill-a-claude)
- [Risoluzione Problemi](#risoluzione-problemi)
- [Licenza](#licenza)

## Che Cos'è

Delegate to Codex è un plugin e una skill per Claude Code.

In parole semplici: insegna a Claude Code quando e come chiedere al Codex CLI di
fare lavoro pesante o specializzato in un'esecuzione separata.

Usalo quando un task è grande, ripetitivo, oppure richiede capacità di Codex
come:

- generare o modificare immagini
- creare file Word, Excel o PowerPoint
- controllare un browser per screenshot, scraping o test di app locali
- lavorare con GitHub
- lavorare con Google Drive, Docs, Sheets o Slides
- leggere o preparare email Gmail
- controllare Google Calendar
- creare o modificare design Canva
- costruire o testare frontend
- processare molti file o svolgere lavoro bulk ad alto consumo di token

Claude Code resta l'assistente principale. Codex è il worker che Claude può
chiamare quando il lavoro è più adatto a un agente da riga di comando separato.

## Requisiti

Ti servono:

- Claude Code installato.
- Codex CLI installato.
- Codex CLI autenticato con:

```bash
codex login
```

Puoi accedere con un account ChatGPT. Non serve billing API OpenAI per il normale
login via ChatGPT.

## Installazione

### Opzione 1: Installazione Dal Marketplace Claude Code

Scrivi questi comandi dentro la chat di Claude Code:

```text
/plugin marketplace add ai-ghostwriter/codex-coprocessor
/plugin install delegate-to-codex@codex-coprocessor
```

Dopo l'installazione, Claude Code può caricare la skill `/delegate-to-codex`
quando la citi in un prompt.

### Opzione 2: Installazione Manuale

Clona il repository:

```bash
git clone https://github.com/ai-ghostwriter/delegate-to-codex.git /path/to/delegate-to-codex
```

Crea la cartella delle skill di Claude Code, se non esiste:

```bash
mkdir -p ~/.claude/skills
```

Crea un symlink della skill dentro Claude Code:

```bash
ln -s /path/to/delegate-to-codex/skills/delegate-to-codex ~/.claude/skills/delegate-to-codex
```

Controlla il symlink:

```bash
ls -l ~/.claude/skills/delegate-to-codex
```

## Come Usarla Nei Prompt

Non devi eseguire Codex a mano.

Nel tuo normale messaggio dentro la chat di Claude Code, includi
`/delegate-to-codex` e spiega cosa vuoi ottenere. Lo slash command si scrive
dentro la chat, come parte della richiesta.

Esempi da copiare:

```text
Controlla queste tre cartelle e sistema tutto quello che non va. Fai eseguire
tutto il lavoro pesante a Codex, usa /delegate-to-codex.
```

```text
Genera le immagini di copertina per questo libro e salvale nella cartella
output, usa /delegate-to-codex.
```

```text
Crea una presentazione PowerPoint da questo report ed esporta il file .pptx
finale, usa /delegate-to-codex.
```

Claude Code deciderà se ha senso delegare, preparerà il prompt per Codex,
eseguirà `codex exec` e poi rivedrà il risultato.

## Cosa Insegna La Skill A Claude

La skill include:

- quando tenere il lavoro in Claude Code
- quando delegare il lavoro a Codex
- come eseguire `codex exec` in modo sicuro
- come chiedere a Codex di scrivere file reali, non solo testo finale in chat
- quali skill o plugin Codex usare per i task comuni

File di riferimento utili:

- [SKILL.md](skills/delegate-to-codex/SKILL.md)
- [CAPABILITIES.md](skills/delegate-to-codex/CAPABILITIES.md)
- [EXAMPLES_KDP.md](skills/delegate-to-codex/EXAMPLES_KDP.md)

## Risoluzione Problemi

Se Claude dice che Codex non è autenticato, esegui questo nel terminale:

```bash
codex login
```

Se un comando `codex exec` resta bloccato, assicurati che termini con:

```bash
< /dev/null
```

Questo impedisce a Codex di restare in attesa di altro input.

Se Codex scrive solo un breve messaggio finale invece di creare un file, chiedi a
Claude di dire a Codex esattamente dove scrivere il file. L'opzione
`--output-last-message` salva solo il messaggio finale di Codex.

Se Codex non riesce a scrivere fuori dalla cartella del progetto, di solito è la
sandbox sicura che sta facendo il suo lavoro. L'accesso completo alla macchina va
usato solo per task locali fidati.

## Licenza

[MIT](LICENSE)
