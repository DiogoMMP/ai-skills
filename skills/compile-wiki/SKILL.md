---
name: compile-wiki
description: Compile/update the markdown wiki/ of a knowledge base created by the knowledge-base skill — research mode (summarize raw/ sources into concept articles) or code mode (fold notes/, git commits, GitHub issues, and pull requests into decision/subsystem articles, plus regenerate an always-current wiki/Estrutura.md project structure and domain-model overview). Cross-links articles and keeps wiki/index.md current. Use when the user wants to compile, refresh, sync, or update their wiki, process newly added sources/notes, or asks to "run the ingest".
allowed-tools: Read, Write, Edit, Glob, Grep, Bash(find *), Bash(git log *), Bash(git show *), Bash(git rev-parse *), Bash(gh repo view *), Bash(gh issue list *), Bash(gh pr list *), AskUserQuestion
---

# Compile wiki

Turns whatever's pending — external sources or your own notes/commits — into the compiled
markdown wiki in `wiki/`, incrementally. Behavior branches on the KB's mode.

## Steps

1. **Find the knowledge base.** If the user gave a path, use it. Otherwise look in the current
   directory (and immediate subdirectories) for a `wiki/` next to a `KB_GUIDE.md`. If exactly
   one candidate is found, confirm it with the user before touching anything. If none or several
   are found, ask for the path.

2. **Read `KB_GUIDE.md`'s frontmatter** to get `mode` (`research` or `code`) and `topic`. If the
   file has no frontmatter, treat it as `research` mode for backward compatibility.

3. **Load shared context.** Read `wiki/index.md` (current map of content). If `wiki/_geral/`
   exists (a directory link into a general/shared vault), skim its `index.md` too, so you can
   link out to existing general concepts instead of duplicating them locally.

4. Follow the steps for the KB's mode:

### Research mode

a. Read `wiki/sources.md` if it exists — a manifest of every raw source, a one-line summary,
   which wiki article(s) cover it, and when it was last compiled. Create it (with a header row)
   if it doesn't exist yet.

b. List every file under `raw/` recursively (skip its `README.md`). A source is *pending* if it
   has no entry in `wiki/sources.md`, or if its mtime is newer than the "last compiled" date
   recorded there. If nothing is pending, say so and stop.

c. For each pending source:
   - Read it in full. For very large files, read enough to summarize accurately and note in
     `wiki/sources.md` that it was only partially read.
   - Decide which existing concept article it belongs to, or whether it warrants a new one.
     Prefer extending an existing article over fragmenting the wiki.
   - Write or update that concept article under `wiki/`: synthesize, don't copy-paste. Cite the
     source back with a relative link into `raw/` (e.g. `[source](../raw/paper.pdf)`).
   - **Write in the language of the source material**, not the language of this instruction file
     or of the chat. If sources are mixed, match whichever language dominates this KB's sources so
     far (check `wiki/sources.md` / existing articles) rather than switching per-source; ask the
     user once, on the first source, if it's genuinely ambiguous. Keep technical/domain terms,
     proper nouns, API and library names, and established jargon in their original language even
     when the surrounding prose is translated — don't force a translation nobody in the field uses.
   - Cross-link related articles with Obsidian-style `[[Article Name]]` links. If a concept
     already has an article in `wiki/_geral/`, link there (`[[_geral/Article Name]]`) instead of
     duplicating it.
   - Embed any associated images from `raw/` with a relative markdown image link — don't copy
     the bytes.
   - Add/update the source's row in `wiki/sources.md`: path, one-line summary, article(s) it
     feeds, today's date.

### Code mode

a. **Regenerate the structure overview.** Rewrite `wiki/Estrutura.md` from scratch every run
   (unlike everything else in this mode, this isn't incremental — it should always reflect the
   project as it is right now):
   - Folder/module layout: a short annotated tree of the meaningful directories (skip
     `node_modules/`, build output, etc.).
   - Tech stack: infer from manifest files present (`package.json`, `pyproject.toml`,
     `go.mod`, `Cargo.toml`, etc.) — languages, frameworks, key dependencies.
   - Entry points: main scripts, servers, CLI commands — wherever execution starts.
   - Domain model: if `KB_GUIDE.md`'s `domain_model` frontmatter field is set, read whatever it
     points to (files/folder or description) and summarize the key entities and how they
     relate. Render it as a small Mermaid class or ER diagram when the model is concrete enough
     (Obsidian renders Mermaid natively); otherwise a short prose summary is fine. Leave this
     section out entirely if `domain_model` is empty — don't invent one.
   - Make sure `wiki/index.md` links `Estrutura.md` first, as the "start here" entry.

b. **Process pending notes.** Glob `notes/*.md`. A note is *pending* if its frontmatter is
   missing or doesn't say `status: compiled`. For each pending note:
   - Read it. Decide which existing wiki article (by decision/subsystem) it belongs to, or
     whether it warrants a new one.
   - Write or update that article: synthesize the note's content into it, don't paste it
     verbatim. Cross-link related articles with `[[Article Name]]` (link into `wiki/_geral/`
     when the concept already lives there). Write in the language the note itself is written in
     (same rule as research mode) — technical terms stay as-is.
   - Rewrite the note's frontmatter to `status: compiled`, `compiled: <today's date>`, and
     `articles:` listing what it fed. Leave the note's body untouched.

c. **Walk repo activity since the last compile.** Read `last_compiled_at` from `KB_GUIDE.md`'s
   frontmatter.
   - If empty (first run), ask the user how far back to look (e.g. all history, last N months,
     since a given tag/date) and use that as the starting point instead of assuming.

   **Commits**: `git log --since="<last_compiled_at>" --oneline` (or the range from the
   first-run answer). Skim messages; use `git show <sha> --stat` when a message alone doesn't
   make the scope clear. Skip trivial commits (typos, formatting, dependency bumps).

   **Issues and pull requests**: first check `gh` is usable — `gh repo view` succeeds and the
   repo has a GitHub remote. If not, skip this part and note in the final report that
   issues/PRs weren't checked (missing/unauthenticated `gh` or no GitHub remote).

   Otherwise:
   - `gh issue list --state all --json number,title,body,state,updatedAt,url --limit 200`
   - `gh pr list --state all --json number,title,body,state,mergedAt,updatedAt,url --limit 200`

   Keep only entries updated after `last_compiled_at` (or everything, on a first run per the
   user's chosen range). A merged PR's description/discussion is often the best source for
   "why" — prefer it over the raw commit list when both cover the same change, to avoid
   documenting the same thing twice.

   For everything gathered (commits, issues, PRs), fold a short synthesized note into the
   relevant wiki article — create one if none fits — citing back: a commit hash (e.g.
   `` `abc1234` ``) for commits, or `#123` for issues/PRs.

   After processing, set `last_compiled_at` in `KB_GUIDE.md`'s frontmatter to the current UTC
   timestamp.

   If there's nothing new anywhere — no pending notes, no new commits, no new issues/PRs — say
   so and stop. Don't rewrite the wiki when there's nothing pending.

5. **Refresh `wiki/index.md`** once everything pending is processed: make sure every
   concept/article is listed and grouped sensibly, and remove stale entries for articles that no
   longer exist.

6. **Report back**: what was processed (sources/notes/commits/issues/PRs), which articles were
   created vs. updated, and flag anything skipped (unreadable formats, datasets too large to
   read directly — research mode may need a dedicated tool in `tools/` instead; or issues/PRs
   skipped because `gh` wasn't usable). In code mode, confirm `wiki/Estrutura.md` was refreshed.

## Notes

- Safe to re-run any time — only what's pending gets touched.
- No RAG/vector DB needed at small-to-medium scale: the index files plus direct reads of
  relevant articles are enough context to work from.
- Never edit files inside `raw/` (research mode) — it's the append-only source of truth. In
  code mode, never rewrite a note's body, only its frontmatter status.
- Never write into `wiki/_geral/` — it's a directory link into a general vault maintained by its
  own `compile-wiki` run. Read and link to it, don't edit through it.
- `gh` is optional for code mode: if it's not installed/authenticated or there's no GitHub
  remote, still process notes and git log — don't block the whole run on it.
- Language is not fixed by this skill or by the English `SKILL.md`/README files of this plugin —
  those document the tool, not the wiki it produces. The wiki follows the source material (see
  step 4's language rule); once a KB has settled on one, stay consistent across articles rather
  than drifting per session.
