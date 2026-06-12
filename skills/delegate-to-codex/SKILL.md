---
name: delegate-to-codex
description: Use when offloading work from Claude Code to the Codex CLI - heavy-token or bulk tasks, large-codebase analysis, image generation, .docx/.xlsx/.pptx file creation, browser automation, GitHub operations, Google Drive/Docs/Sheets/Slides, Gmail, Calendar, Canva, frontend development, CI debugging, or any task that maps to a Codex plugin/skill the user has installed. Codex runs as a separate agent, so this conserves Claude's context window.
---

# Delegate to Codex

Offload work from Claude Code to the [Codex CLI](https://github.com/openai/codex)
(a separate coding agent). Codex is not just a token sink: it has its own plugin
and skill ecosystem. Before delegating, pick the right capability so Codex uses
the best tool for the job - see [`CAPABILITIES.md`](CAPABILITIES.md) for the full
domain to skill map.

> **Capabilities depend on what you have installed.** The map in
> `CAPABILITIES.md` lists Codex plugins/skills (imagegen, documents, browser,
> gmail, canva, and more). Codex can only do what is installed and authenticated on your
> machine. Treat the map as a menu, not a guarantee.

## When to delegate

**Token / context reasons:**
- Bulk repetitive operations (process N files, generate N variants)
- Large-codebase analysis that would exhaust Claude's context
- Multi-step tasks that don't need Claude's judgment at each step
- Anything that would fill a large fraction of the context window

**Capability reasons** (Codex has a skill Claude lacks): image generation, Office
file creation, browser automation, GitHub/Google/Gmail/Calendar/Canva
integrations, frontend scaffolding. See [`CAPABILITIES.md`](CAPABILITIES.md).

## What to keep in Claude

- Decisions requiring user approval or judgment
- Tasks that depend on the current conversation's memory
- Final review and synthesis of Codex output
- Routing logic (which skill to invoke next)

## How to invoke: Bash

Codex's stdio MCP server is not injected into Claude Code as a native tool, so the
reliable path is to shell out to `codex exec`.

### Safe default (recommended)

Run with the standard workspace sandbox: Codex can read/write inside the project
directory but not touch the wider system.

```bash
codex exec \
  --skip-git-repo-check \
  --sandbox workspace-write \
  -C /path/to/project \
  -o /tmp/codex_result.txt \
    "PROMPT - tell Codex which skill to use and what to produce" < /dev/null

cat /tmp/codex_result.txt
```

### Full access - trusted local use only

> **Security warning.** The flags below disable Codex's sandbox and approval
> prompts, giving the agent full read/write access to your machine and the ability
> to run arbitrary commands non-interactively. Only use this on a machine you
> control, with prompts you trust, never on untrusted input or in CI with secrets.
> Prefer the safe default above and escalate only when a task genuinely needs
> access outside the project directory.

```bash
codex exec \
  --skip-git-repo-check \
  --dangerously-bypass-approvals-and-sandbox \
  -s danger-full-access \
  -C /path/to/project \
  -o /tmp/codex_result.txt \
  "PROMPT" < /dev/null
```

### Key flags

- `< /dev/null` - required; prevents stdin from blocking the run
- `-o file` / `--output-last-message file` - captures only the agent's final message
- `-C path` / `--cd path` - sets the working directory for file access
- `--json` - adds streaming JSONL, useful for parsing mid-run
- `--sandbox` / `-s` - `read-only` | `workspace-write` | `danger-full-access`

**For file output:** tell Codex explicitly to write to a path (`write the result
to /tmp/output.json`). Do not rely on `-o` for files - it only captures the
agent's final message, not file writes.

## Plugin activation

Codex activates the right skill when you **name it in the prompt**
(e.g. "Use the `imagegen` skill to ..."). See `CAPABILITIES.md` for the names and
ready-to-adapt example prompts.

## Quick diagnosis

| Symptom | Fix |
|---|---|
| `codex exec` blocks forever | Add `< /dev/null` to the command |
| Output file is empty | Tell Codex to write explicitly to the path; don't use `-o` for files |
| Auth error | Run `codex login` in the terminal |
| Skill not activating | Name the skill explicitly in the prompt |
| Wrong working directory | Check `-C /absolute/path` is correct |
| Permission denied writing outside project | You're in `workspace-write`; that's expected - escalate only if needed |

## Reference

- [`CAPABILITIES.md`](CAPABILITIES.md) - domain to skill map with example prompts
- [`EXAMPLES_KDP.md`](EXAMPLES_KDP.md) - a real-world case study: using Codex as a
  worker in a book-publishing pipeline

## Requirements

- [`codex`](https://github.com/openai/codex) CLI in PATH, authenticated
  (`codex login`)
- The Codex plugins/skills you intend to invoke, installed locally
