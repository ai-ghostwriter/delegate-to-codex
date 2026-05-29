# Codex Capability Map

Organized by domain. For each delegation, name the skill in the prompt so Codex
activates the right tool. Example prompts are generic and ready to adapt —
replace paths and content with your own.

> **Availability:** these capabilities come from Codex plugins/skills installed on
> your machine. Names follow the `plugin:skill` convention where applicable. If a
> skill isn't installed, that capability won't activate.

---

## Image generation & editing
**Skill:** `imagegen`

Generate raster images from text prompts, edit/inpaint existing images, apply
style/color/composition transforms, save as PNG/JPEG.

```
Use the imagegen skill to generate a cover image.
Style: minimalist, dark green and gold. Title text: 'Deep Work'.
Save as /path/to/output/cover_v1.png
```

---

## Document creation (.docx)
**Skill:** `documents`

Create formatted Word documents from structured content, edit with tracked
changes, apply styles/headings/tables/lists, render and verify visually.

```
Use the documents skill to create a Word document from /path/to/content.json.
Apply H1 for section titles, H2 for subsections.
Save as /path/to/output/report.docx
```

---

## Spreadsheet creation (.xlsx)
**Skill:** `spreadsheets`

Create and format spreadsheets, read Excel/CSV data, build pivot tables, charts
and formulas, analyze tabular data.

```
Use the spreadsheets skill to read /path/to/export.xlsx.
Extract all rows where column "value" > 1000.
Write results to /tmp/filtered.xlsx with columns: name, value, rank
```

---

## Presentation creation (.pptx)
**Skill:** `presentations`

Create PowerPoint files from content, apply themes/layouts/per-slide images, edit
existing decks.

```
Use the presentations skill to create a 10-slide deck
summarizing /path/to/report.md. Save as /tmp/report.pptx
```

---

## Browser automation
**Plugin:** `browser-use` | **Skill:** `browser-use:browser`

Open URLs in a real browser, click/type/scroll/fill forms, screenshot pages,
scrape JS-rendered content, test local apps at `localhost:PORT`.

```
Use the browser skill to open https://example.com/product/123.
Screenshot the page to /tmp/page.png and extract: title, price, rating.
Write to /tmp/data.json
```

---

## GitHub operations
**Plugin:** `github` | **Skills:** `github:github`, `github:yeet`, `github:gh-fix-ci`, `github:gh-address-comments`

Read/create/update issues and PRs, commit/push/open draft PRs (`yeet`), debug
failed Actions CI (`gh-fix-ci`), apply PR review comments (`gh-address-comments`),
search code/repos/users.

```
Use the github:yeet skill to commit all changes in /path/to/project,
push to origin, and open a draft PR titled "feat: add parser".
Branch: feat/parser
```

---

## Google Drive, Docs, Sheets, Slides
**Plugin:** `google-drive` | **Skills:** `google-drive:google-drive`, `google-drive:google-docs`, `google-drive:google-sheets`, `google-drive:google-slides`, `google-drive:google-drive-comments`

Search/list Drive files, read content (Docs, Sheets, Slides, PDFs), create/edit
Docs/Sheets/Slides, add/resolve comments.

```
Use google-drive:google-sheets to read the file named "export.xlsx" in Drive.
Extract columns name, value, rank. Write as JSON to /tmp/data.json
```

---

## Gmail
**Plugin:** `gmail` | **Skills:** `gmail:gmail`, `gmail:gmail-inbox-triage`

Read/search emails, filter by sender/subject/date, draft and send replies,
triage the inbox, extract attachments.

```
Use gmail:gmail-inbox-triage to scan unread emails from the last 7 days.
Flag anything from a known vendor list as HIGH priority.
Write a summary to /tmp/inbox_triage.md
```

---

## Google Calendar
**Plugin:** `google-calendar` | **Skills:** `google-calendar:google-calendar`, `...-daily-brief`, `...-free-up-time`, `...-group-scheduler`, `...-meeting-prep`

Read upcoming events, create/update events, find free slots, generate a
daily/weekly brief, prepare meeting notes and agenda.

```
Use google-calendar:google-calendar-daily-brief
to generate today's schedule summary. Write to /tmp/daily_brief.md
```

---

## Canva design
**Plugin:** `canva` | **Skills:** `canva:canva-branded-presentation`, `canva:canva-resize-for-all-social-media`, `canva:canva-translate-design`

Create brand-kit presentations, resize a design for all social formats, translate
an existing Canva design.

```
Use canva:canva-resize-for-all-social-media on design [CANVA_URL].
Generate: Instagram square, Instagram story, Facebook cover, LinkedIn banner.
```

---

## Frontend development
**Plugin:** `build-web-apps` | **Skills:** `build-web-apps:frontend-app-builder`, `...:frontend-testing-debugging`, `...:react-best-practices`, `...:shadcn`, `...:stripe-best-practices`, `...:supabase-postgres-best-practices`

Build React/Next apps from scratch, debug UI/browser test failures, apply
React/Next best practices, manage shadcn/ui components, integrate Stripe, design
Supabase/Postgres schemas.

```
Use build-web-apps:frontend-app-builder to create a React data-viewer app.
It reads /path/to/data.json and renders each record as a card.
Stack: Vite + React + Tailwind. Save to /path/to/app/
```

---

## Meta: skill & plugin creation
**Skills:** `skill-creator`, `plugin-creator`, `skill-installer`

Scaffold new Codex plugins, create/update Codex skills, install skills from
curated repos.

```
Use skill-creator to write a new Codex skill named "report-generator"
that takes data.json and a template and produces a formatted report.
```

---

## OpenAI docs lookup
**Skill:** `openai-docs`

Retrieve current OpenAI API docs; answer questions about models, endpoints,
pricing, limits; look up SDK usage.

```
Use openai-docs to look up the current pricing and size options for image
generation.
```

---

## Knowledge graph
**Skill:** `graphify`

Build a semantic knowledge graph from a codebase or document set, query
relationships between files/concepts/functions, generate cluster reports.

```
Use graphify to build a knowledge graph of /path/to/project,
then query "which files depend on config.json".
```
