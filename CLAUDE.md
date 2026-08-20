# gradData — PUBLIC repo

# ⛔ EVERYTHING HERE IS PUBLIC, PUBLISHED, AND PERMANENT ⛔

`github.com/studycourts/gradData` — live at **studycourts.github.io/gradData/**

A file committed here is public the moment it is pushed. Deleting it later does
not unpublish it: git history stays readable and the page may already be indexed
or archived. **Treat every write here as irreversible publication.**

---

## Never in this repo

- Solution keys, answers, worked solutions, or anything derived from
  `../GradMethods/` keys
- Grading rubrics or internal grading notes
- Assignments not yet released to students
- Speaker notes, or any deck built with the `teach` profile
- Student work of any kind, anonymized or not
- Anything copied from `../GradMethods/` without Rebecca approving that specific
  file

## Push policy

**Every push requires explicit approval, every time.** Not a standing approval,
not "she approved the last one." Stage, show the diff, wait.

---

## Layout

```
gradData/
├── .github/ISSUE_TEMPLATE/   <- already set up
├── data/                     <- teaching datasets students load in assignments
├── docs/                     <- GitHub Pages source (this is what is served)
├── pages/                    <- page sources
├── gradData.Rproj
└── .gitignore
```

Pages is served from `docs/`, so **anything written into `docs/` goes live on the
next push.** Treat that directory with particular care.

For per-page YAML, heading structure, and CSS specifics, `taBot/GradMethods_knowledge_base.md`
in `../GradMethods/` is the authoritative reference — consult it before authoring
or revising a page rather than relying on this file.

------------------------------------------------------------------------

## Standalone reference page conventions

Pages in `pages/` (multicollinearity, outliers, and future ones — see
`../GradMethods/taBot/GradMethods_knowledge_base.md` for the candidate list per
course) follow conventions distinct from homework keys, since these are
public-facing:

- `author: "Rebecca Gill"` — her real name, not "Gill's Version" (that's a key
  convention, not a public-document one).
- `self_contained: true` — required for GitHub Pages; embeds all assets in one
  HTML file.
- `code_folding: hide` always — readers can reveal code but it's hidden by
  default (pages are read, not worked through).
- Custom `knit:` function renders to `../docs/` — see Site build below.
- **Voice is more informal than homework keys** — closer to how she talks in
  class. Distinctive feature: a Socratic Q&A rhythm using `::: question` boxes
  voiced as an imagined, slightly exasperated student ("Wait. What? You're
  kidding."), answered patiently in the following prose. Preserve this when
  drafting or revising a page here.

There is currently **no LICENSE file.** Worth adding — CC BY-NC-SA or similar. On
a public repo of teaching material, absent an explicit license, reuse terms are
whatever a visitor assumes them to be.

---

## ⚠️ Dataset change protocol

Datasets in `data/` are loaded by assignments, keys, and slide code chunks over in
`../GradMethods/`. Renaming a variable, recoding a level, or moving a file
**breaks student-facing material in a repo that has no visibility into this one** —
and the breakage surfaces when a student hits it the night before a deadline.

Before changing anything in `data/`:

1. Grep `../GradMethods/` for the dataset filename and for every variable name
   being altered. Include `.qmd`, `.Rmd`, `.R`, and the `701setup.R`-style setup
   scripts.
2. List every assignment, key, deck, and tutorial that references it.
3. Report that list **before** making the change.
4. If it proceeds, update all dependents in the same session and re-render to
   confirm the code still executes.

Never change a dataset in isolation. There is no such thing as a local change here.

---

## Published decks

Only ever the `public` profile build, which has speaker notes stripped. See
`../GradMethods/CLAUDE.md` for the render commands. Before any push:

```bash
../GradMethods/scripts/check-deck.sh docs/slides/*.html
```

Zero is the only acceptable result. Public builds are self-contained single HTML
files, so each deck is one file with no accompanying assets directory.

## Handouts

Guided-notes handouts are published here alongside the decks, and this is the
artifact that matters most for accessibility — it is the version a screen reader
can actually work with. **Posted before the class session, not after.**

A handout is a scaffold, not a transcript: structure, equations, code, and key
terminology, without narration or interpretation. Full rationale in
`../CLAUDE.md`; preserve that distinction when generating one.

## Accessibility

The standards in `../CLAUDE.md` apply with extra force here — this is the public
face of the courses and the tutorials get used unsupervised. Real heading
structure, alt text on every image, MathJax rather than images of equations,
header rows on tables, no colour-only encoding in figures.

## Site build — `docs/` is GENERATED. Never hand-edit it.

Every `.Rmd` in `pages/` (including `page_template.Rmd`, so this propagates to
future pages) carries a custom `knit:` function in its YAML:

```r
knit: (function(input, ...) {
    rmarkdown::render(input, output_dir = "../docs")
  })
```

Knitting a page in `pages/` writes its self-contained HTML straight into
`docs/`. There is no separate copy step — knitting *is* publishing to the file
GitHub Pages serves.

**`pages/` is source. `docs/` is output.**

- To change site content, edit the `.Rmd` in `pages/` and re-knit it — the
  custom `knit:` function handles landing the output in `docs/`.
- Never edit an `.html` in `docs/` directly — the change is silently lost the
  next time that page's `.Rmd` is knitted.
- If an `.html` in `docs/` has no corresponding `.Rmd` in `pages/`, say so rather
  than assuming; it may be an orphan from an earlier workflow.
- It's still manual in the sense that each page has to be opened and knitted
  individually — there's no single command that re-knits everything in `pages/`
  in one pass. That's the piece actually worth scripting, if anything: iterate
  `pages/*.Rmd`, knit each, report which `docs/` outputs changed.

Always preview locally before proposing a push. Since approval is required
anyway, there is no cost to checking first.
