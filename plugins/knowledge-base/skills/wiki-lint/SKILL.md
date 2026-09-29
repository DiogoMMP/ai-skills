---
name: wiki-lint
description: Health-check the compiled wiki/ of a knowledge base — broken internal links, articles orphaned from wiki/index.md, stale references to files/decisions that no longer exist, and dangling manifest entries. Reports findings and asks before fixing anything. Use when the user wants to lint, clean up, audit, or check the health/consistency of their wiki.
allowed-tools: Read, Grep, Glob, Edit, Write, AskUserQuestion
---

# Wiki lint

The "lint" step from Karpathy's
[LLM Knowledge Bases](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) note:
periodically sweep the wiki for inconsistencies instead of letting them accumulate silently.
Reports first, fixes only what the user approves.

## Steps

1. **Find the knowledge base.** Same discovery as `compile-wiki`: look for `wiki/` next to
   `KB_GUIDE.md` nearby, ask if not found or ambiguous. Read the frontmatter for `mode`.

2. **Check for broken internal links.** Grep every `.md` file under `wiki/` (excluding
   `wiki/_geral/`, which belongs to another vault) for `[[Article Name]]` wikilinks and relative
   markdown links. For each, verify the target exists — as a `.md` file somewhere under `wiki/`
   (including `wiki/_geral/` for links that intentionally point there) for wikilinks, or as a
   real path for relative links. Flag anything that doesn't resolve.

3. **Check for orphaned articles.** List every article under `wiki/` (excluding `index.md`,
   `Estrutura.md`, `sources.md`, `README.md`, and anything under `_geral/`). Flag any that
   aren't linked from `wiki/index.md` or from any other article — they exist but nothing points
   to them.

4. **Check for dangling manifest entries.**
   - Research mode: rows in `wiki/sources.md` whose listed article(s) no longer exist, or whose
     source file is gone from `raw/`.
   - Both modes: notes in `notes/` marked `status: compiled` whose listed `articles:` no longer
     exist, and (research mode) notes whose `source:` points to a `raw/` file that doesn't exist.

5. **Check for stale references (code mode).** For articles that cite a file path (e.g. in
   backticks), spot-check whether that path still exists in the repo (`Glob`). Treat this as a
   low-confidence heuristic, not a hard rule — code moves around legitimately — and flag
   candidates rather than asserting they're wrong.

6. **Report findings** as a clear list grouped by category, even if some categories are empty.
   Don't touch anything yet.

7. **Ask the user which fixes to apply**, e.g.: relink or remove broken links, link orphaned
   articles into `wiki/index.md` (or confirm they should stay unlinked / be deleted), fix or
   remove dangling manifest rows, and note stale-reference candidates for the user to confirm
   before touching the article text.

8. **Apply only the approved fixes**, then report what changed.

## Notes

- Never delete an article without explicit confirmation — orphaned isn't the same as wrong, it
  might just be missing a link from `index.md`.
- Never touch `wiki/_geral/` — it's a directory link into another vault, maintained by its own
  `compile-wiki`/`wiki-lint` run.
- This complements `compile-wiki`, it doesn't replace it: run this periodically for upkeep, not
  as part of every compile.
