# outputs/

Ad-hoc artifacts generated while querying the knowledge base: Q&A answers, Marp slide decks,
matplotlib/chart images, one-off reports.

When an output turns out to be durably useful, "file it back" into `../wiki/` (as its own
article or folded into an existing one) instead of leaving it stranded here.

## Where things go

**This structure is not fixed.** Before putting anything here, look at what `outputs/` and
`../wiki/` (and `../raw/`) look like *right now* and decide where it fits. Whenever a folder
starts to get ambiguous — mixed content, too many loose files, an unclear home for the next
output — **reorganise it** (move the files, fix the index table, tell the user) instead of
adding to the pile.

Starting guidelines:

- Organise by **context, mirroring the areas `../wiki/` already uses** (a course, a project, a
  topic) — one taxonomy, not two. Don't invent a grouping the sources don't have.
- An output lives in the folder of the area it came from, e.g. `outputs/<area>/`.
- When an area has its own sub-divisions in `../wiki/` or `../raw/` (a challenge, a module, a
  phase), mirror them one level down: `outputs/<area>/<sub-area>/`.
- A set of files that belong together (a script + the diagram it generates + its data) gets its
  own subfolder. Don't scatter it by file type.
- Something that fits no area goes in `outputs/geral/`.
- If the wiki has no areas (a single-topic KB), keep `outputs/` flat.
- Past two levels is a sign to promote the good outputs into `wiki/`, not to nest deeper.
- Name new files `YYYY-MM-DD-slug.<ext>` so they sort by date.
- Create folders when the first output needs them — don't pre-create empty ones.

## Index

Every output gets a row here, added by whoever generates it, and kept in sync whenever files
are moved. The type (slides, chart, answer, report…) is a column, not a folder.

| Date | File | Type | Question / goal | Based on | Status |
| --- | --- | --- | --- | --- | --- |

- **Based on** — the wiki articles and/or `raw/` files that back it, or `fora da KB` /
  `outside the KB` when it comes from general knowledge or a web search.
- **Status** — `one-off`, or `promoted → wiki/<article>.md` once filed back.
