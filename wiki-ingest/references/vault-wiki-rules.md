# Vault Wiki Rules

## Authority and write boundary

- The authoritative local policy is `pkm/AGENTS.md`.
- Edit only `wiki/` by default.
- Treat these paths as read-only: `raw/`, `tmp/`, `utility/`, `journals/`, `GTD/`, `Root.md`, `.obsidian/`, `.trash/`.
- Never move, delete, reformat, or rename source files during ingestion.
- A Codex attachment may be used as a read-only source only when the user explicitly names it as an exception source. Cite it with `（例外元ソース）`; do not move, copy, normalize, or edit the attachment.

## Wiki structure

- `wiki/index.md`: wiki index.
- `wiki/log.md`: chronological operation log.
- `wiki/summaries/`: source summary pages.
- `wiki/concepts/`: reusable concept pages.
- `wiki/entities/`: named entities such as people, tools, companies, and projects.
- `wiki/queries/`: saved question-answer pages.

## Standard wiki page format

Use this format for normal wiki pages:

```markdown
---
type: concept | entity | summary | query | comparison
aliases:
  - Alias 1
updated: YYYY-MM-DD
tags: [tag1, tag2]
---
## 概要
...
## 詳細
...
## ソース
- [[Source file|Display name]]
## 関連
- [[Related page]]
```

Formatting constraints:

- Do not put a blank line immediately after the closing frontmatter `---`.
- Do not put blank lines immediately before or after Markdown headings.
- Do not use horizontal rules in body text.
- Do not add H1 headings; the filename is the title.
- Use YAML list form for `aliases`.
- If there are no aliases, use `aliases: []`.
- Do not include the filename itself as an alias.

## Normal ingest

When the user asks to ingest a file from `raw/` or `tmp/`, or explicitly authorizes a Codex attachment as an exception source:

1. Read the target source directly.
2. Proceed without confirmation unless the user asks to review decisions interactively.
3. Create `wiki/summaries/[YYYY-MM-DD-slug].md`.
4. Prefer a date from the source file, article, or document; use the current date only when the source date cannot be determined.
5. List concepts and entities that appear in the source.
6. For concepts/entities, always write from the original `raw/`, `tmp/`, or explicitly authorized attachment exception source. Do not derive concept/entity claims from the summary.
7. Update existing pages rather than creating duplicates.
8. Create new concept/entity pages only when there is no existing page.
9. If new source information conflicts with an existing page, keep both and mark the contradiction explicitly.
10. Append `- ingest | [source title]` to `wiki/log.md`.

## Deep ingest

Use deep ingest for explicit `ingest-deep` requests or equivalent language.

- Do all normal ingest steps.
- Search existing `wiki/concepts/` and `wiki/entities/` for aliases, overlapping pages, and likely related concepts.
- Update or create all relevant durable pages, not just the source summary.
- Add related links between affected pages when the relationship is clear from the source or existing wiki.
- Keep summaries as discovery aids; do not use them as the main basis for concepts/entities.
- Treat impact on 10 to 15 pages from one rich source as normal.

## Paper PDF exception

For PDFs under `raw/pdf/papers/`:

- Do not create the usual `wiki/summaries/[YYYY-MM-DD-slug].md` summary.
- Use `wiki/summaries/papers-notes/[PDF filename with .md extension]`.
- If the corresponding `.md` exists, preserve its frontmatter and update only the body as needed.
- If it does not exist, read `utility/templater/tmpl_literature_note.md` as a read-only template and create the new paper note under `wiki/summaries/papers-notes/`.
- Do not force paper-note frontmatter into the normal `type/aliases/updated/tags` wiki format.
- When updating concept/entity pages, cite the original PDF directly rather than the paper-note summary.

## Log maintenance

When appending to `wiki/log.md`:

- Use date headings in the form `## [[YYYY_MM_DD]]`.
- Keep date headings in descending order, newest first.
- If today's heading exists, append under it.
- If today's heading does not exist, insert it in the correct descending position.
- Log every wiki operation after it changes wiki files.

## Source links

- Use Obsidian wiki links for internal sources and related pages.
- Prefer links to the original source file in `raw/` or `tmp/` for concept/entity `## ソース` sections.
- For explicitly authorized attachment exceptions, cite the absolute attachment path and add `（例外元ソース）`.
- Use summaries for source overview and navigation, but not as the sole evidence for durable concept/entity claims.

## Final report

After ingestion, report:

- Summary page created or updated.
- Concept/entity pages created or updated.
- Log entry added.
- Any contradictions, skipped candidates, unreadable files, or wiki gaps.
- Whether an attachment exception was used, and that `raw/` or `tmp/` remains preferable for long-term source preservation.
