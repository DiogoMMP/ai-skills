<!-- kb:start -->
## Knowledge base — {{TOPIC}}

This repo keeps a knowledge base of its decisions and structure. Before answering questions about *why* something is the way it is, or how the project is organised, use it — don't answer from memory. `KB_GUIDE.md` has the full conventions.

### How to look things up

1. **Start with the wiki.** Read `wiki/index.md`, then the relevant articles. For the current folder layout, tech stack, entry points and domain model, read `wiki/Estrutura.md`.
2. **Go to the code, `notes/`, or git history only when the wiki isn't enough**: you need the exact implementation, the original wording of a note, or the commit/PR where something changed. The code is the truth for *what it does now*; the wiki is the truth for *why*.
3. `notes/` are the author's raw scratch notes; `wiki/` is where they've been compiled.

### Don't invent

- Ground answers about decisions and rationale in the wiki, and cite the article you used.
- If the answer isn't in the knowledge base, you may search online or answer from general knowledge — but say explicitly that it is **not based on the knowledge base**. Never present a guess as a recorded decision.
- If the wiki and the code disagree, say so — the wiki may be stale (suggest `compile-wiki`).

### Writing markdown

Write every `.md` file with **no hard line breaks inside a sentence or paragraph**: one paragraph (or list item) per line, however long. Don't wrap at 80/100 columns. The editor or viewer that opens the file adapts the line width; manual wrapping only makes diffs noisy and breaks the layout on other screen sizes. Line breaks belong only between paragraphs, list items, headings, table rows and code blocks.

### Maintenance

The wiki is maintained by the `compile-wiki` skill, not by hand. When you make a decision worth remembering, drop a note in `notes/` (see `notes/README.md`) instead of editing `wiki/`.
<!-- kb:end -->
