# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

`openprecedent-arch` is a **doctrine-only** repo. No code, no schemas, no runbooks, no build system, no tests. Implementations live in sibling repos:

- `openprecedent-backend` — API, agent harness, read-side repositories, treatment-signal specs (`docs/specs/`)
- `openprecedent-pipeline` — bulk ETL, embedding pipeline, treatment-edge loaders (`scripts/`)

Two files matter:

- `specs/world-model-architecture.md` — the canonical architectural spec. **Markdown is canonical.**
- `index.html` — a self-contained static viewer for the spec. It is a renderer, not a source of truth.

`README.md` is a pointer to the spec; keep it short.

## Editing rules specific to this repo

- **The spec is the source of truth for sibling-repo work.** When implementation in `openprecedent-backend` or `openprecedent-pipeline` drifts from this doctrine, either fix the implementation or update this spec — don't let drift live silently (README.md:20).
- **Preserve the priority-ordered reading structure.** The spec opens with a "How to read this" section that numbers sections in priority order (Who → What/Why/Four Layers → Abstention → How We Plan on Using It → Caveats). New sections must slot into that ordering, and the "How to read this" index must be updated to match.
- **Cross-repo links in the spec point to GitHub URLs on `thecloudgeek/openprecedent-backend` and `openprecedent-pipeline`**, not relative paths — sibling repos are not checked out alongside this one. When adding a reference, use the same `https://github.com/thecloudgeek/...` form already in the file.
- **The four "Abstention as a First-Class Output" rules are binding** on every capability, interface, and proactive signal described downstream. If you add a rule, add it there — don't scatter principles across sections. The fourth rule (*Validate against deployment distribution*) introduces the `candidate` vs `validated` tier states that other specs reference; keep that vocabulary consistent.
- **`index.html` must be regenerated/updated to match `specs/world-model-architecture.md` when the spec changes meaningfully.** It is a hand-authored self-contained renderer (inlined CSS, Google Fonts via CDN, no build step) — open it directly in a browser to preview. There is no generator script; edits to the HTML are manual.

## Commit / PR conventions

Recent history uses Conventional Commits scoped to `docs` or `docs(world-model)`:

- `docs(world-model): add Path B + value-validation sequencing note (#6)`
- `docs(world-model): add validation-discipline rule + candidate-tier state (#5)`
- `docs: tiered treatment signals, abstention-as-output, UPL framing, deadline-watching to v1`

Match that style. PRs against `main` are the normal flow.

## What not to do

- Don't add a build system, package.json, linter config, or CI for prose/HTML — none exists by design.
- Don't introduce code, schemas, or operational state here; that belongs in the sibling repos.
- Don't create new top-level docs unless the spec genuinely needs to be split; prefer adding a section to `world-model-architecture.md`.
