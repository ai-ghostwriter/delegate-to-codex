# Delegate to Codex 🛰️

> A [Claude Code Skill](https://docs.claude.com/en/docs/claude-code/skills) that
> teaches Claude **when and how to hand work off to the Codex CLI** — to save
> context, or to use a capability Claude doesn't have.

*[🇮🇹 Versione italiana](README.it.md)*

Claude Code is great at judgment, routing and review. The [Codex
CLI](https://github.com/openai/codex) is a separate agent with its own plugin
ecosystem — image generation, Office file creation, browser automation, Google
Workspace, Gmail, Canva, frontend scaffolding, and more. This skill lets Claude
treat Codex as a **worker process**: offload the heavy or specialized task, keep
Claude's context window for the thinking.

## What it gives you

- **A decision rule** for what to delegate vs. keep in Claude.
- **The reliable invocation pattern** (`codex exec` via Bash) with a safe-by-default
  sandbox and an explicit, well-warned full-access mode.
- **A capability map** ([`CAPABILITIES.md`](CAPABILITIES.md)) — domain → Codex
  skill → ready-to-adapt example prompt.
- **A real case study** ([`EXAMPLES_KDP.md`](EXAMPLES_KDP.md)) — Codex as a worker
  in a publishing pipeline (Claude orchestrates, Codex renders).

## The core idea

```
  Claude Code  ──delegates──►  codex exec  ──►  result file / artifact
   (orchestrator,                (worker:           │
    judgment, review)             generation,       ▼
                                  rendering,    Claude reviews & routes
                                  bulk work)
```

## Quick start

```bash
# Safe default: Codex can read/write inside the project only
codex exec \
  --skip-git-repo-check --sandbox workspace-write \
  -C /path/to/project -o /tmp/result.txt \
  "Use the imagegen skill to generate a logo. Save as output/logo.png" < /dev/null

cat /tmp/result.txt
```

See [`SKILL.md`](SKILL.md) for the full reference and
[`CAPABILITIES.md`](CAPABILITIES.md) for what Codex can do.

## ⚠️ Security note

This skill documents a `--dangerously-bypass-approvals-and-sandbox -s
danger-full-access` mode that gives the agent full machine access and runs
commands without approval. **Use it only on a machine you control, with prompts
you trust — never on untrusted input or in CI with secrets.** The recommended
default is `--sandbox workspace-write`, which confines Codex to the project
directory. The skill leads with the safe option and treats full access as a
deliberate escalation.

## Requirements

- [`codex`](https://github.com/openai/codex) CLI in PATH, authenticated (`codex login`)
- The Codex plugins/skills you intend to invoke, installed locally. The capability
  map describes a rich install; your mileage depends on what you've added.

## Installing as a Claude Code skill

Claude Code discovers skills in your **central skills directory**,
`~/.claude/skills/`. The folder name there must match the skill name
(`delegate-to-codex`). Two ways to set it up:

### Option A — Clone directly into the central skills directory

Simplest. The repo *is* the installed skill.

```bash
git clone https://github.com/ai-ghostwriter/delegate-to-codex.git \
  ~/.claude/skills/delegate-to-codex
```

Update later with `git -C ~/.claude/skills/delegate-to-codex pull`.

### Option B — Keep the repo separate, link it with a symlink

Best if you keep all your skills/projects in one development folder and want the
central directory to hold only links. Edit in one place, `git pull` in one place,
and the symlink keeps `~/.claude/skills/` in sync.

```bash
# clone wherever you develop
git clone https://github.com/ai-ghostwriter/delegate-to-codex.git ~/dev/delegate-to-codex

# link it into the central skills directory
ln -s ~/dev/delegate-to-codex ~/.claude/skills/delegate-to-codex
```

Verify it resolves: `ls -l ~/.claude/skills/delegate-to-codex`.

## License

[MIT](LICENSE)
