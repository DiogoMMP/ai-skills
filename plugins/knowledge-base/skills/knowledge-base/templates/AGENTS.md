<!-- kb:start -->
## Knowledge base — {{TOPIC}}

This directory is a knowledge base. Before answering anything about {{TOPIC}}, use it — don't
answer from memory. `KB_GUIDE.md` has the full conventions.

### How to look things up

1. **Start with the wiki.** Read `wiki/index.md`, then the articles it points to that are
   relevant. The wiki is the compiled, cross-linked view of the sources — it's the default
   place to answer from.
2. **Go to `raw/` only when the wiki isn't enough**: you need the exact wording, a figure, a
   table, an image, or detail the wiki summarised away. Use `wiki/sources.md` to find which
   raw file backs an article. Never edit anything in `raw/`.
3. `notes/` holds the author's own thoughts and AI answers they saved. They are the author's
   view, not a source — AI-authored notes are unverified.

### Don't invent

- Ground every answer in the wiki (and `raw/` when you consulted it). Cite the article or
  source file you used.
- If the answer isn't in the knowledge base, you may search online or answer from general
  knowledge — but say explicitly that it is **not based on the knowledge base's sources**.
  Never present it as if it came from them, and never fill a gap with a guess.
- If sources disagree or are unclear, say so instead of picking one silently.

### Maintenance

The wiki is maintained by the `compile-wiki` skill, not by hand. Don't edit `wiki/` to answer
a one-off question — put one-off answers in `outputs/`.
<!-- kb:end -->
