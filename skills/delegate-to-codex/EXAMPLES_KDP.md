# Case study: Codex as a worker in a publishing pipeline

This appendix shows how `delegate-to-codex` can be used in a publishing workflow
where Claude Code orchestrates and Codex does the heavy lifting: image
generation, document rendering, bulk file processing, and live web checks.
It's a concrete illustration of the patterns in the main
[`SKILL.md`](SKILL.md); adapt the paths to your own project.

> The examples below use the **full-access** invocation because the pipeline runs
> on a trusted local machine. For untrusted contexts use the safe default
> (`--sandbox workspace-write`) documented in `SKILL.md`.

## Pattern: generate a cover image and save to disk

```bash
codex exec \
  --skip-git-repo-check \
  --dangerously-bypass-approvals-and-sandbox -s danger-full-access \
  -C /path/to/book-project \
  --output-last-message /tmp/codex_result.txt \
  "Use the imagegen skill to generate a book cover.
   Style from data/brand_theme.json.
   Title from data/brief.json field 'title'.
   Save as output/COVER/cover_v1.png" < /dev/null
```

## Pattern: create a .docx from JSON content

```bash
codex exec \
  --skip-git-repo-check \
  --dangerously-bypass-approvals-and-sandbox -s danger-full-access \
  -C /path/to/book-project \
  --output-last-message /tmp/codex_result.txt \
  "Use the documents skill. Read data/bookPayload.json.
   Create a print-ready Word document with proper heading styles.
   Save as output/manuscript.docx" < /dev/null
```

## Pattern: scrape a live product listing

```bash
codex exec \
  --skip-git-repo-check \
  --dangerously-bypass-approvals-and-sandbox -s danger-full-access \
  --output-last-message /tmp/codex_result.txt \
  "Use the browser skill to open https://www.example.com/dp/PRODUCT_ID.
   Extract: title, rank, price, review count, bullet points.
   Write JSON to /tmp/listing_data.json" < /dev/null
```

## Pattern: Claude prepares the prompt, Codex writes, Claude reviews

The highest-leverage pattern: Claude builds a precise prompt with a script, Codex
does the token-heavy generation, Claude does quality control on the result.

```bash
# Step 1 - Claude compiles the prompt (deterministic, scripted)
python scripts/generate_chapter_prompt.py \
  --chapter N --brief data/brief.json --outline data/outline.json \
  --output /tmp/chapter_N_prompt.md

# Step 2 - Codex writes the chapter
codex exec \
  --skip-git-repo-check \
  --dangerously-bypass-approvals-and-sandbox -s danger-full-access \
  -C /path/to/book-project \
  --output-last-message /tmp/chapter_N_result.txt \
  "You are a professional non-fiction author.
   Read /tmp/chapter_N_prompt.md - full instructions for this chapter.
   Follow every instruction exactly. Output ONLY valid JSON. Start with { end with }.
   Save to output/chapters/chNN.output.json" < /dev/null

# Step 3 - Claude does QC: schema, real sources, language, forbidden terms

# Step 4 - If under target, Codex expands
codex exec \
  --skip-git-repo-check \
  --dangerously-bypass-approvals-and-sandbox -s danger-full-access \
  -C /path/to/book-project \
  --output-last-message /tmp/chapter_N_expand.txt \
  "Read output/chapters/chNN.output.json.
   Current: ACTUAL words. Target: TARGET words. Add DELTA words expanding thin sections.
   Do NOT change structure or existing content. Output ONLY JSON. Overwrite the file." < /dev/null
```

## Pattern: process a large file set

```bash
codex exec \
  --skip-git-repo-check \
  --dangerously-bypass-approvals-and-sandbox -s danger-full-access \
  -C /path/to/project \
  --output-last-message /tmp/codex_result.txt \
  "Read every file in output/chapters/.
   For each, extract the word count from the 'content' field.
   Write a summary table to /tmp/word_counts.json
   Format: [{chapter_id, word_count, target_met}]" < /dev/null
```

## Key facts (this setup)

| Property | Value |
|----------|-------|
| Auth | ChatGPT Plus (CLI login, not API billing) |
| File access | Full read/write with `danger-full-access` |
| Plugin activation | Name the skill in the prompt |
| Cold-start overhead | Non-trivial token cost per `exec` call - batch where possible |
| Multi-turn | Not supported via a single `exec`; each call is independent |

The division of labor: Codex generates and renders; Claude decides, routes, and
reviews. Each `exec` call is stateless, so pass all needed context in the prompt
or in files Codex can read via `-C`.
