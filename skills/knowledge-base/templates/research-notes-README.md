# notes/

Your own material, kept apart from `../raw/` (what the sources say). One file per note, not one
growing file. Typical contents: a thought about something you read, a question, an answer from
an AI (Claude, ChatGPT, …) you want to keep. `compile-wiki` folds pending notes into `../wiki/`
as your view, clearly separate from what the sources state.

Each note starts with a small frontmatter block:

```markdown
---
status: pending
kind: thought
source: raw/some-paper.pdf
from: ChatGPT
---

Whatever you want to write, unstructured. Paste AI answers as they are.
```

- `status` — `pending` until `compile-wiki` processes it.
- `kind` (optional) — `thought` (default: your own idea or reaction), `ai-response` (text
  produced by an AI) or `question` (something still open).
- `source` (optional) — the `raw/` file the note is about, so `compile-wiki` attaches it to the
  right article instead of guessing.
- `from` (optional, for `ai-response`) — which AI produced it.

`ai-response` notes are treated as unverified: they end up in the wiki attributed to the AI, never
as established fact, unless a `raw/` source backs them up.

After `compile-wiki` processes a note, it rewrites the frontmatter to `status: compiled`, adds
`compiled: <date>` and `articles:` (what it fed), and leaves the rest as you wrote it. Anything
without `status: compiled` (including notes with no frontmatter) is pending.
