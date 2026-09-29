---
name: capture-note
description: Quickly capture a scratch note (a decision, a "why did I do it this way", a gotcha) into a knowledge base's notes/ folder with the right frontmatter, ready for compile-wiki to fold in later. Use when the user wants to jot down a thought, log a decision, or note something to remember later for their knowledge base / wiki.
allowed-tools: Write, Read, Glob, Bash(mkdir *), AskUserQuestion
---

# Capture note

Frictionless note-taking: write down a thought now, let `compile-wiki` organize it later. This
is the write side of the `notes/` convention used by the `knowledge-base` skill (both modes: in research mode it holds your own notes and
reactions alongside `raw/`).

## Steps

1. **Find the knowledge base.** If the user gave a path, use it. Otherwise look in the current
   directory (and immediate subdirectories) for a `wiki/` next to a `KB_GUIDE.md`. If none is
   found, ask where the note should go, or offer to run the `knowledge-base` skill first if
   there isn't one yet.

2. **Ensure `notes/` exists.** If it doesn't, create it with a short `README.md` explaining the
   convention (a `status: pending`/`status: compiled` frontmatter marker per note, as described
   below).

3. **Get the note's content.**
   - If invoked with arguments (`$ARGUMENTS`), use that text directly.
   - Otherwise, if the user's message already contains the thing to remember (e.g. "lembra-te
     que decidimos X por causa de Y"), use that content directly — don't make them repeat
     themselves.
   - Only ask if neither is available.

4. **Write the note** to `notes/<YYYY-MM-DD>-<slug>.md`, where `<slug>` is a short kebab-case
   summary of the content (append `-2`, `-3`, etc. if that filename already exists today):

   ```markdown
   ---
   status: pending
   ---

   <the note content, lightly cleaned up but not rewritten>
   ```

   **Research-mode KBs** (`mode: research` in `KB_GUIDE.md`) take three optional extra fields,
   added only when they're evident from what the user said — don't ask for them:
   `kind:` (`thought` by default; `ai-response` when the text is pasted from an AI;
   `question` for something still open), `source:` (the `raw/` file it's about) and `from:`
   (which AI, for `ai-response`). Paste AI answers verbatim rather than "cleaning them up". If
   `notes/` has to be created in a research KB, use the wording of
   `knowledge-base/templates/research-notes-README.md` for its `README.md`.

5. **Confirm** with one line — the note's filename and a note that `compile-wiki` will fold it
   into the wiki later. Don't over-narrate; this should feel instant.

## Notes

- Don't organize, categorize, or cross-link here — that's `compile-wiki`'s job when it runs
  later. This skill only captures.
- Don't touch any other file in `notes/` or anything in `wiki/`.
