# MSK Fellowship Manual — project notes for Claude

This project is the source for the UW–Madison MSK Imaging and Intervention Fellowship Manual, rebuilt as a navigable documentation site. It replaces a long Google Docs / Word manual that fellows reportedly don't read because the dense format makes finding anything tedious.

The audience is incoming and current MSK fellows. Secondary audience: faculty, residents rotating on the service, the program coordinator who will edit the manual going forward.

## Stack

- **MkDocs** + **Material for MkDocs** theme
- **Markdown** source in `docs/`
- **GitLab CI** (`.gitlab-ci.yml`) builds and deploys to UW DoIT GitLab Pages on every push to `main`
- **Site config** in `mkdocs.yml` (theme, navigation, extensions, custom CSS hookup)
- **Custom CSS** in `docs/stylesheets/extra.css` (UW red palette, callout styling)

When making changes that affect the navigation structure or available pages, update `mkdocs.yml`. When adding new Markdown features (admonitions, tabs, etc.), confirm the relevant extension is enabled in `mkdocs.yml` first.

## Source material

The original manual lives at `_source/2025-2026_Fellowship_manual.md`. Treat this as the **authoritative source for facts** (procedures, policies, phone numbers, schedules) but **not for structure or wording**. The whole point of this project is to do better than the original on both.

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
2. **Daily Operations** — `daily-workflow.md`, `communication.md`
3. **Clinical Rotations** — one page per service: `procedures-uh.md`, `float.md`, `sports-spine.md`, `ed-bone.md`, `emh.md`, `smc.md`
4. **On Call** — `taking-call.md`
5. **Schedule & Time Off** — `clinical-schedule.md`, `time-off.md`
6. **Conferences & Academics** — `conferences.md`, `academic-time.md`, `meetings.md`, `meeting-dates.md`, `research.md`
7. **Logistics & Reference** — `logistics.md`, `wimr-contrast.md`, `rr-priorities.md`, `discrepancies.md`, `spine-workflow.md`, `contacts.md`, `wellness.md`

When adding, removing, or renaming a page, update `mkdocs.yml` to match.

## Workflow conventions

- Commit in small chunks (one section migrated = one commit). Don't batch many sections into one commit.
- Use descriptive commit messages: "migrate East Madison rotation" not "update".
- Use `mkdocs serve` for live local preview during editing.
- Push to `main` deploys automatically — there's no staging environment.
- The site is published at the GitLab Pages URL (publicly accessible at the moment; may move to private later if any privileged content is added).

## What's already done

Every page in the nav exists as a file in `docs/`, but most are stubs awaiting migration from the source manual. State as of the latest pass:

- **Functional drafts** (establish the pattern, will get further edits): `index.md`, `daily-workflow.md`, `communication.md`, `procedures-uh.md`, `taking-call.md`, `time-off.md`.
- **Stubs** (skeleton + headings only, content not yet migrated): everything else — `clinical-schedule.md`, `academic-time.md`, `conferences.md`, `meetings.md`, `meeting-dates.md`, `research.md`, `float.md`, `sports-spine.md`, `ed-bone.md`, `emh.md`, `smc.md`, `logistics.md`, `wimr-contrast.md`, `rr-priorities.md`, `discrepancies.md`, `spine-workflow.md`, `contacts.md`, `wellness.md`.

`procedures-uh.md` remains the reference example for rotation pages. Match its pattern when filling in the rotation stubs (`float.md`, `sports-spine.md`, `ed-bone.md`, `emh.md`, `smc.md`): TL;DR → checklist or timeline → collapsibles for details → related links at the bottom.

## Next up

Pick up in **Daily Operations** (`daily-workflow.md`, `communication.md`).
