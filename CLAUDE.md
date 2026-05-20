# MSK Fellowship Manual — project notes for Claude

This project is the source for the UW–Madison MSK Imaging and Intervention Fellowship Manual, rebuilt as a navigable documentation site. It replaces a long Google Docs / Word manual that fellows reportedly don't read because the dense format makes finding anything tedious.

The audience is incoming and current MSK fellows. Secondary audience: faculty, residents rotating on the service, the program coordinator who will edit the manual going forward.

## Stack

- **MkDocs** + **Material for MkDocs** theme
- **Markdown** source in `docs/`
- **Site config** in `mkdocs.yml` (theme, navigation, extensions, custom CSS hookup)
- **Custom CSS** in `docs/stylesheets/extra.css` (UW red palette, callout styling, page-specific font sizes)
- **Two CI pipelines** publish on every push to `main`:
  - `.gitlab-ci.yml` → UW DoIT GitLab Pages (NetID-gated, internal preview)
  - `.github/workflows/deploy.yml` → GitHub Pages (the **public** site)

When making changes that affect the navigation structure or available pages, update `mkdocs.yml`. When adding new Markdown features (admonitions, tabs, etc.), confirm the relevant extension is enabled in `mkdocs.yml` first.

## Editing and publishing

### Where this lives

- **Canonical repo (where you edit):** UW DoIT GitLab — <https://git.doit.wisc.edu/ross22/msk-fellowship-manual>
- **Mirror (auto-pushed from GitLab):** GitHub — <https://github.com/UW-MSK-Rads/msk-fellowship-manual>
- **Public site:** <https://uw-msk-rads.github.io/msk-fellowship-manual/>
- **NetID-gated mirror site:** the GitLab Pages URL exposed by DoIT (same content, requires UW SSO)

Edit on GitLab. GitHub is downstream — **don't push to GitHub directly**; you'll diverge from GitLab and confuse the mirror.

### Local setup (new machine / forgotten environment)

```bash
git clone https://git.doit.wisc.edu/ross22/msk-fellowship-manual.git
cd msk-fellowship-manual
pip install mkdocs-material
```

That's the only dependency — Material for MkDocs pulls in MkDocs itself.

### Previewing locally

```bash
mkdocs serve
```

Opens <http://127.0.0.1:8000> with live reload. Use this every time before pushing — the GitHub Action runs `mkdocs build --strict`, which fails the deploy on any broken internal link.

### Making a change and publishing

1. Edit the relevant `docs/*.md` (or `mkdocs.yml` for nav changes).
2. `mkdocs serve` and spot-check.
3. Commit and push to GitLab:

   ```bash
   git add <files>
   git commit -m "Short imperative description"
   git push origin main
   ```

4. Everything else is automatic:
   - GitLab CI builds and deploys the NetID-gated site
   - GitLab's **push mirror** replicates the commit to GitHub
   - GitHub Actions builds with `mkdocs build --strict` and deploys to GitHub Pages
   - Public site refreshes within ~2 minutes

### If the public site doesn't update

Walk down this list:

1. **GitLab mirror status** — DoIT GitLab → Settings → Repository → expand *Mirroring repositories*. "Last update" should be within a minute of your push. Errors usually mean the GitHub PAT expired or was revoked. To fix: generate a new fine-grained PAT on GitHub (Settings → Developer settings → Personal access tokens, scoped to this repo, **Contents: read+write**), paste it into the GitLab mirror's password field, hit the refresh icon to retry.
2. **GitHub Actions run** — <https://github.com/UW-MSK-Rads/msk-fellowship-manual/actions>. Click into any red run for the log. The most common failure is `mkdocs build --strict` rejecting a broken internal link — fix the link in `docs/`, push again.
3. **GitHub Pages config** — Repo → Settings → Pages → Source must be **"GitHub Actions"** (not a branch).

### Who can edit

- Editing access is managed in **GitLab → Settings → Members**. Add a UW NetID to grant push access.
- The GitHub mirror authenticates with a PAT issued by Ross22 (UW-MSK-Rads). If that account departs UW, the mirror needs to be re-pointed at a new PAT holder.

## Source material

The original manual lives at `_source/2025-2026 Fellowship manual.md`. Treat this as the **authoritative source for facts** (procedures, policies, phone numbers, schedules) but **not for structure or wording**. The whole point of this project is to do better than the original on both.

The `_source/` directory is excluded from the published site. Don't copy paragraphs verbatim from it — paraphrase, restructure, cut.

## Editorial philosophy

The original manual fails because nobody reads dense walls of text. The redesign succeeds when a fellow can find the answer to "when do I need to arrive on procedures days" in under 10 seconds.

**Cut aggressively.** A 2-sentence statement of a rule beats a 6-sentence paragraph that hedges it. If a paragraph doesn't change what a fellow does, consider cutting it or moving it to a collapsible. Test every paragraph: "Will a fellow take a different action knowing this?" If no → cut or collapse.

**Voice: direct second-person imperative.** "Arrive by 7:15" not "fellows are expected to arrive by approximately 7:15 AM." No hedging language ("usually," "generally," "in most cases") unless the variability is genuinely important and consequential.

**Front-load the actionable.** Every page has a TL;DR callout at the top with the 30-second version. Details and edge cases live below, often in collapsibles. The fellow scanning during a coffee break should be able to absorb the essentials from the TL;DR alone.

**Cross-link generously.** When a page mentions a concept covered elsewhere (time off policy, communication policy, a specific person, a contact), link to that page. This is what makes the manual feel like a wiki rather than a stack of documents.

**Don't manufacture content.** If a topic is sparse in the source manual, the new page should be sparse too. Don't pad. Empty space is fine; padding is not.

## Markdown conventions

Established patterns in this manual — match them when adding content:

- `!!! tldr "TL;DR"` — red-bordered "if you read nothing else" box at the top of major pages
- `!!! pro-tip "Pro tip"` — gold-bordered tip box for non-obvious wisdom
- `!!! warning "Title"` — for things that have consequences if ignored (compliance rules, hard deadlines)
- `!!! note "Title"` — informational asides
- `!!! danger "Title"` — for the rare truly-don't-screw-this-up content
- `??? abstract "Title"` — collapsed-by-default detail block (click to expand)
- `=== "Tab name"` — tabbed content, use sparingly and only when the tabs are genuinely parallel options

Tables are great for: schedules, contacts, comparisons, anything matrix-shaped. Avoid tables for content that wants to be prose.

Numbered lists are great for: sequences with strict order (morning workflow, escalation paths). Bulleted lists for everything else. Don't number a list whose items aren't actually ordered.

The `procedures-uh.md` page is the current reference for what a well-structured rotation page looks like. Match its pattern for other rotation pages: TL;DR → checklist or timeline → collapsibles for details → related links at the bottom.

## Navigation structure

`mkdocs.yml`'s `nav:` block is the source of truth. Current top-level sections:

1. **Home** (`index.md`) — quick-reference cards, the six-services overview, what's new this year
2. **Daily Operations** — `daily-operations.md`
3. **Clinical Rotations** — one page per service: `procedures-uh.md`, `float.md`, `sports-spine.md`, `ed-bone.md`, `emh.md`, `smc.md`
4. **On Call** — `taking-call.md`
5. **Schedule & Time Off** — `schedule.md`
6. **Conferences & Academics** — `conferences.md`, `academic-time.md`
7. **Reference** — `wimr-contrast.md`, `rr-priorities.md`, `discrepancies.md`, `spine-workflow.md`, `wellness.md`

When adding, removing, or renaming a page, update `mkdocs.yml` to match — and grep the rest of `docs/` for cross-links to the renamed/removed page.

## Workflow conventions

- Commit in small chunks (one section migrated = one commit). Don't batch many sections into one commit.
- Use descriptive commit messages: "migrate East Madison rotation" not "update".
- Push to `main` deploys automatically — there's no staging environment, so preview locally with `mkdocs serve` first.

## What's already done

Snapshot as of the last pass — re-check the actual files if much time has passed.

- **Functional drafts** (establish the pattern, will get further edits): `index.md`, `daily-operations.md`, `procedures-uh.md`, `taking-call.md`, `schedule.md`, `academic-time.md`, `conferences.md`, `wimr-contrast.md`, `spine-workflow.md`, `sports-spine.md`.
- **Stubs** (skeleton + headings only, content not yet migrated): `float.md`, `ed-bone.md`, `emh.md`, `smc.md`, `rr-priorities.md`, `discrepancies.md`, `wellness.md`.

`procedures-uh.md` remains the reference example for rotation pages. Match its pattern when filling in the rotation stubs (`float.md`, `ed-bone.md`, `emh.md`, `smc.md`): TL;DR → checklist or timeline → collapsibles for details → related links at the bottom.

## Next up

Pick up in the **Reference** section (e.g., `rr-priorities.md`, `discrepancies.md`, `wellness.md`) or fill in the remaining rotation stubs.
