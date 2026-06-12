# Delegate to Codex

*[Versione italiana](README.it.md)*

## What This Is

Delegate to Codex is a Claude Code plugin and skill.

In plain language: it teaches Claude Code when and how to ask the Codex CLI to do
heavy or specialized work in a separate run.

Use it when a task is large, repetitive, or needs Codex capabilities such as:

- generating or editing images
- creating Word, Excel, or PowerPoint files
- controlling a browser for screenshots, scraping, or local app testing
- working with GitHub
- working with Google Drive, Docs, Sheets, or Slides
- reading or drafting Gmail messages
- checking Google Calendar
- creating or editing Canva designs
- building or testing frontend apps
- processing many files or doing token-heavy bulk work

Claude Code remains the main assistant. Codex is the worker that Claude can call
when the job is better suited to a separate command-line agent.

## Requirements

You need:

- Claude Code installed.
- Codex CLI installed.
- Codex CLI logged in with:

```bash
codex login
```

You can log in with a ChatGPT account. You do not need OpenAI API billing for
the normal ChatGPT login flow.

## Installation

### Option 1: Install From The Claude Code Marketplace

Type these commands inside the Claude Code chat box:

```text
/plugin marketplace add ai-ghostwriter/codex-coprocessor
/plugin install delegate-to-codex@codex-coprocessor
```

After installation, Claude Code can load the `/delegate-to-codex` skill when you
mention it in a prompt.

### Option 2: Manual Installation

Clone the repository:

```bash
git clone https://github.com/ai-ghostwriter/delegate-to-codex.git /path/to/delegate-to-codex
```

Create the Claude Code skills folder if it does not exist:

```bash
mkdir -p ~/.claude/skills
```

Symlink the skill into Claude Code:

```bash
ln -s /path/to/delegate-to-codex/skills/delegate-to-codex ~/.claude/skills/delegate-to-codex
```

Check the symlink:

```bash
ls -l ~/.claude/skills/delegate-to-codex
```

## How To Use In Prompts

You do not need to run Codex yourself.

In your normal Claude Code chat message, include `/delegate-to-codex` and explain
what you want done. The slash command is typed inside the chat box as part of
your request.

Copy-paste examples:

```text
Check these three folders and fix everything that is wrong. Have Codex do the
heavy work, use /delegate-to-codex.
```

```text
Generate the cover images for this book and save them in the output folder, use
/delegate-to-codex.
```

```text
Create a PowerPoint summary from this report and export the final .pptx file,
use /delegate-to-codex.
```

Claude Code will decide whether delegation makes sense, prepare a Codex prompt,
run `codex exec`, and then review the result.

## What The Skill Teaches Claude

The skill includes:

- when to keep work in Claude Code
- when to delegate work to Codex
- how to run `codex exec` safely
- how to ask Codex to write real files, not just final chat text
- which Codex skills or plugins match common tasks

Useful reference files:

- [SKILL.md](skills/delegate-to-codex/SKILL.md)
- [CAPABILITIES.md](skills/delegate-to-codex/CAPABILITIES.md)
- [EXAMPLES_KDP.md](skills/delegate-to-codex/EXAMPLES_KDP.md)

## Troubleshooting

If Claude says Codex is not logged in, run this in your terminal:

```bash
codex login
```

If a `codex exec` command hangs, make sure the command ends with:

```bash
< /dev/null
```

That prevents Codex from waiting for more input.

If Codex writes only a short final message instead of a file, ask Claude to tell
Codex exactly where to write the file. The `--output-last-message` option saves
only Codex's final chat message.

If Codex cannot write outside the project folder, that is usually the safe
sandbox doing its job. Full-machine access should be used only on trusted local
tasks.

## License

[MIT](LICENSE)
