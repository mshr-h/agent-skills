---
name: wiki-ingest
description: Maintain the Obsidian Vault wiki by ingesting new source files from raw/ or tmp/ into wiki/, or by ingesting a Codex attachment only when the user explicitly names it as an exception source. Use when the user says ingest, ingest-deep, インジェストして, 深く取り込んで, or asks Codex to add source material into the Vault wiki while preserving the rule that only wiki/ may be edited.
---

# Wiki Ingest

## Required context

Before acting, read `references/vault-wiki-rules.md`. Treat `/PATH/TO/VAULT/AGENTS.md` as the authoritative policy if it differs from this skill.

## Safety boundary

Confirm target sources exist under `/PATH/TO/VAULT/raw/` or `/PATH/TO/VAULT/tmp/`. Never modify, move, delete, or normalize files under `raw/`, `tmp/`, `utility/`, `journals/`, `GTD/`, `Root.md`, `.obsidian/`, or `.trash/`. Write only under `/PATH/TO/VAULT/wiki/`.

## Attachment source exception

Use a Codex attachment path as a source only when the user explicitly says to treat that attachment as the source. Treat attachment paths as read-only: do not move, delete, normalize, copy into the Vault, or edit them. If the user does not explicitly authorize the attachment exception, ask them to place the source under `raw/` or `tmp/`, or plan the ingest on that assumption.

When using this exception, mark every source citation as an exception source, for example:

```markdown
- `/Users/[user]/.codex/attachments/.../pasted-text.txt`（例外元ソース）
```

Concept and entity pages must still be written from the original attachment text itself, not from the generated summary. In the final report, state that the attachment exception was used and note that long-term source preservation is better handled by placing the source under `raw/` or `tmp/`.

## Mode selection

Use `ingest` for normal ingestion unless the user explicitly says `ingest-deep`, `deep ingest`, `深く取り込んで`, or asks for exhaustive concept/entity maintenance.

Use `ingest-deep` when the user wants a deeper pass. This mode includes normal ingestion and then systematically updates or creates all relevant concept/entity pages from the original source.

## Ingest workflow

1. Read the original source directly from `raw/` or `tmp/`, or from the explicitly authorized attachment exception source.
2. Create or update the appropriate summary page under `wiki/summaries/`.
3. Identify obvious concepts and entities from the source.
4. Update existing concept/entity pages when the source directly adds useful information; create new pages only for clearly reusable knowledge.
5. Preserve contradictions by labeling them instead of deleting either side.
6. Update `wiki/log.md` with an `ingest` entry under the correct date heading.
7. Report changed wiki files, any notable gaps, and whether the attachment source exception was used.

## Ingest-deep workflow

Follow the ingest workflow, then perform a comprehensive concept/entity pass:

1. Build a candidate list of concepts, entities, aliases, and related pages from the original source and existing `wiki/concepts/` and `wiki/entities/`.
2. For each candidate, decide whether to update an existing page or create a new page; avoid duplicates.
3. Write concept/entity content only from the original source, not from the generated summary. For attachment exceptions, the original source is the attachment text.
4. Add source links, related links, aliases, updated dates, and contradiction notes where needed.
5. Expect one source to affect many pages; do not stop after the first few obvious pages if the source has broader reusable knowledge.
6. Update `wiki/log.md` once with the source title, and mention the deep pass in the final report.

## Paper PDF exception

For PDFs under `raw/pdf/papers/`, use `wiki/summaries/papers-notes/[PDF filename with .md extension]` as the summary note instead of `wiki/summaries/[YYYY-MM-DD-slug].md`. Preserve existing paper-note frontmatter. If no corresponding note exists, read `utility/templater/tmpl_literature_note.md` as a read-only template source and create the note under `wiki/summaries/papers-notes/`. Concept/entity pages must still cite the original PDF, not the summary note.
