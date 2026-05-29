# Delegate to Codex 🛰️

> Una [Claude Code Skill](https://docs.claude.com/en/docs/claude-code/skills) che
> insegna a Claude **quando e come passare il lavoro al Codex CLI** — per
> risparmiare contesto, o per usare una capacità che Claude non ha.

*[🇬🇧 English version](README.md)*

Claude Code è bravo in giudizio, routing e revisione. Il [Codex
CLI](https://github.com/openai/codex) è un agente separato con il suo ecosistema
di plugin — generazione immagini, creazione file Office, automazione browser,
Google Workspace, Gmail, Canva, scaffolding frontend e altro. Questa skill
permette a Claude di trattare Codex come un **processo worker**: scarica il task
pesante o specializzato, e tieni la finestra di contesto di Claude per il
ragionamento.

## Cosa ti dà

- **Una regola decisionale** su cosa delegare vs cosa tenere in Claude.
- **Il pattern di invocazione affidabile** (`codex exec` via Bash) con sandbox
  sicura di default e una modalità full-access esplicita e ben avvisata.
- **Una capability map** ([`CAPABILITIES.md`](CAPABILITIES.md)) — dominio → skill
  Codex → prompt d'esempio pronto da adattare.
- **Un caso reale** ([`EXAMPLES_KDP.md`](EXAMPLES_KDP.md)) — Codex come worker in
  una pipeline editoriale (Claude orchestra, Codex renderizza).

## L'idea di fondo

```
  Claude Code  ──delega──►  codex exec  ──►  file risultato / artefatto
   (orchestratore,            (worker:            │
    giudizio, revisione)       generazione,       ▼
                               rendering,     Claude rivede e instrada
                               lavoro bulk)
```

## Avvio rapido

```bash
# Default sicuro: Codex può leggere/scrivere solo dentro il progetto
codex exec \
  --skip-git-repo-check --sandbox workspace-write \
  -C /path/to/project -o /tmp/result.txt \
  "Use the imagegen skill to generate a logo. Save as output/logo.png" < /dev/null

cat /tmp/result.txt
```

Vedi [`SKILL.md`](SKILL.md) per il riferimento completo e
[`CAPABILITIES.md`](CAPABILITIES.md) per cosa sa fare Codex.

## ⚠️ Nota di sicurezza

Questa skill documenta una modalità `--dangerously-bypass-approvals-and-sandbox -s
danger-full-access` che dà all'agente accesso completo alla macchina ed esegue
comandi senza approvazione. **Usala solo su una macchina che controlli, con prompt
di cui ti fidi — mai su input non fidato o in CI con segreti.** Il default
consigliato è `--sandbox workspace-write`, che confina Codex alla cartella di
progetto. La skill mette davanti l'opzione sicura e tratta il full-access come
un'escalation deliberata.

## Requisiti

- CLI [`codex`](https://github.com/openai/codex) in PATH, autenticato (`codex login`)
- I plugin/skill Codex che intendi invocare, installati localmente. La capability
  map descrive un'installazione ricca; ciò che funziona dipende da cosa hai
  aggiunto.

## Installazione come skill di Claude Code

Claude Code scopre le skill nella tua **directory skill centrale**,
`~/.claude/skills/`. Lì il nome della cartella deve combaciare col nome della
skill (`delegate-to-codex`). Due modi per configurarla:

### Opzione A — Clona direttamente nella directory skill centrale

Il più semplice. Il repo *è* la skill installata.

```bash
git clone https://github.com/ai-ghostwriter/delegate-to-codex.git \
  ~/.claude/skills/delegate-to-codex
```

Aggiorni in seguito con `git -C ~/.claude/skills/delegate-to-codex pull`.

### Opzione B — Tieni il repo separato e collegalo con un symlink

Ideale se tieni tutte le skill/progetti in un'unica cartella di sviluppo e vuoi
che la directory centrale contenga solo link. Modifichi in un posto, fai
`git pull` in un posto, e il symlink mantiene `~/.claude/skills/` allineata.

```bash
# clona dove sviluppi
git clone https://github.com/ai-ghostwriter/delegate-to-codex.git ~/dev/delegate-to-codex

# collegalo nella directory skill centrale
ln -s ~/dev/delegate-to-codex ~/.claude/skills/delegate-to-codex
```

Verifica che risolva: `ls -l ~/.claude/skills/delegate-to-codex`.

> 💡 Questa è esattamente la convenzione di `~/.claude/skills/` su questa macchina:
> solo symlink che puntano alle skill mantenute in `Documents/Claude/SKILLS/`.

## Licenza

[MIT](LICENSE)
