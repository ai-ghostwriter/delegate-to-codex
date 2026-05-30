# Delegate to Codex 🛰️

> Un [plugin per Claude Code](https://docs.claude.com/en/docs/claude-code/plugins)
> (e skill) che insegna a Claude **quando e come passare il lavoro al Codex CLI** —
> per risparmiare contesto, o per usare una capacità che Claude non ha.

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
- **Una capability map** ([`CAPABILITIES.md`](skills/delegate-to-codex/CAPABILITIES.md)) —
  dominio → skill Codex → prompt d'esempio pronto da adattare.
- **Un caso reale** ([`EXAMPLES_KDP.md`](skills/delegate-to-codex/EXAMPLES_KDP.md)) —
  Codex come worker in una pipeline editoriale (Claude orchestra, Codex renderizza).

## L'idea di fondo

```
  Claude Code  ──delega──►  codex exec  ──►  file risultato / artefatto
   (orchestratore,            (worker:            │
    giudizio, revisione)       generazione,       ▼
                               rendering,     Claude rivede e instrada
                               lavoro bulk)
```

## Installazione come plugin

Elencato nella marketplace `codex-coprocessor` insieme agli altri strumenti
Claude+Codex.

```text
# dentro Claude Code:
/plugin marketplace add ai-ghostwriter/codex-coprocessor
/plugin install delegate-to-codex@codex-coprocessor
```

## Avvio rapido (il pattern)

```bash
# Default sicuro: Codex può leggere/scrivere solo dentro il progetto
codex exec \
  --skip-git-repo-check --sandbox workspace-write \
  -C /path/to/project -o /tmp/result.txt \
  "Use the imagegen skill to generate a logo. Save as output/logo.png" < /dev/null

cat /tmp/result.txt
```

Vedi [`SKILL.md`](skills/delegate-to-codex/SKILL.md) per il riferimento completo e
[`CAPABILITIES.md`](skills/delegate-to-codex/CAPABILITIES.md) per cosa sa fare Codex.

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

## Installazione manuale (solo skill, senza plugin)

Claude Code scopre anche le skill sciolte in `~/.claude/skills/`. Il nome della
cartella deve combaciare col nome della skill (`delegate-to-codex`).

```bash
git clone https://github.com/ai-ghostwriter/delegate-to-codex.git ~/dev/delegate-to-codex
ln -s ~/dev/delegate-to-codex/skills/delegate-to-codex ~/.claude/skills/delegate-to-codex
```

Verifica che risolva: `ls -l ~/.claude/skills/delegate-to-codex`.

## Licenza

[MIT](LICENSE)
