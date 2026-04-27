# MSK Fellowship Manual

Source for the UW–Madison Musculoskeletal Imaging and Intervention Fellowship Manual.

Built with [MkDocs](https://www.mkdocs.org/) and the [Material](https://squidfunk.github.io/mkdocs-material/) theme. Deployed automatically to GitLab Pages on every commit to the default branch.

## Editing

**For routine edits (recommended):**

1. Browse to the file under `docs/` in the GitLab web UI
2. Click the pencil icon to edit
3. Make your changes (it's just Markdown)
4. Commit with a short description of what you changed
5. Wait ~1–2 minutes — the live site rebuilds automatically

**For larger edits or local preview:**

```bash
# one-time setup
pip install mkdocs-material

# preview locally
mkdocs serve
# open http://127.0.0.1:8000

# commit and push as normal
git add -A
git commit -m "describe your change"
git push
```

## Project structure

```
.
├── docs/                    # all manual content (Markdown)
│   ├── index.md             # home page
│   ├── stylesheets/         # custom CSS
│   └── *.md                 # one file per section
├── mkdocs.yml               # site config and navigation
├── .gitlab-ci.yml           # build/deploy pipeline (don't edit unless you know why)
└── .gitignore
```

## Adding a new page

1. Create a new `.md` file in `docs/` (e.g. `docs/new-section.md`)
2. Add an entry to the `nav:` section of `mkdocs.yml` so it appears in the sidebar
3. Commit — the new page is live

## Markdown conventions used

- `!!! tldr "Title"` — TL;DR callout (UW red)
- `!!! note "Title"` — informational callout
- `!!! warning "Title"` — warning callout
- `!!! pro-tip "Title"` — gold-accented tip
- `??? abstract "Title"` — collapsed-by-default section (click to expand)
- `=== "Tab name"` — tabbed content blocks

See existing pages (especially `procedures-uh.md`) for examples.
