---
mode: research
topic: {{TOPIC}}
language: {{LANGUAGE}}
created: {{DATE}}
---

# Knowledge Base guide — {{TOPIC}}

Conventions for any LLM session working in this directory. Adapted from Andrej Karpathy's
["LLM Knowledge Bases"](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

## Layout

- `raw/` — source material (articles, papers, repos, datasets, images). Read-only; never
  edited by the LLM.
- `notes/` — your own notes (thoughts, questions, answers from other AIs), one file per note
  with `status: pending`/`compiled` frontmatter and optional `kind`, `source`, `from`.
  `compile-wiki` folds them into `wiki/` as your view (AI answers as unverified, attributed),
  distinct from what `raw/` says. Never rewritten by the LLM beyond that frontmatter.
- `wiki/` — the compiled knowledge base: `.md` files with backlinks, organized by concept.
  LLM-maintained; humans mostly just read it (e.g. in Obsidian).
- `outputs/` — one-off generated artifacts (Q&A answers, Marp slides, charts). Promote the
  useful ones into `wiki/`. Organised by the same areas as `wiki/` (one folder per course,
  project or topic; `geral/` for the rest), and indexed in a table in `outputs/README.md`.
  The folder structure is not fixed: check it before adding anything, and reorganise it when
  it gets ambiguous.
- `tools/` — scripts/CLIs that operate on `raw/` and `wiki/` (search, linting, importers).

## Workflow

1. **Ingest** — new source material goes into `raw/` as-is (web clips, PDFs, repos, datasets,
   images). Don't summarize or rewrite it on the way in.
2. **Compile** — incrementally turn `raw/` and pending `notes/` into `wiki/`: write/update summaries, extract
   concepts into their own articles, cross-link related articles, keep `wiki/index.md` current
   as a map of content. This is incremental — re-run it as `raw/` grows, don't start from
   scratch each time.
3. **Q&A** — answer questions against the wiki by reading `wiki/index.md` plus whatever
   articles are relevant; fall back to `raw/` or a live search when the wiki doesn't have the
   answer yet. No need for a vector DB at small-to-medium scale — index files + direct reads are
   enough.
4. **Output** — render answers as markdown, Marp slide decks, or images (e.g. matplotlib) into
   `outputs/` rather than only replying in chat, so results are viewable in Obsidian and
   reusable later. File genuinely useful outputs back into `wiki/`. Put each output in the
   folder of the area it belongs to and add a row to the table in `outputs/README.md` (see
   there for the rules). Look at the current structure first; if it has become ambiguous,
   reorganise it and update the table.
5. **Lint** — periodically sweep `wiki/` for inconsistencies, missing data (impute via web
   search where appropriate), and interesting new connections worth turning into their own
   article. Treat this as ongoing maintenance, not a one-time cleanup.
6. **Tools** — build small scripts in `tools/` as needed (e.g. a search CLI over `wiki/`) and
   prefer using them over ad-hoc re-reading of every file once the wiki gets large.

## Ground rules

- The wiki is written and maintained by the LLM. Humans view it, they don't hand-edit it.
- Keep `wiki/index.md` accurate — it's the entry point for every future session.
- When in doubt about whether something belongs in `wiki/` vs `outputs/`: durable knowledge →
  `wiki/`; one-off answer to a specific question → `outputs/` (and maybe later promoted).
