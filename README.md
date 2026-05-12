# Open Precedent — Architecture

Doctrine for Open Precedent. Implementations live in sibling repos:

- [`openprecedent-backend`](https://github.com/thecloudgeek/openprecedent-backend) — API, agent harness, read-side repositories, treatment-signal specs
- [`openprecedent-pipeline`](https://github.com/thecloudgeek/openprecedent-pipeline) — bulk ETL, embedding pipeline, treatment-edge loaders

## Start here

[`specs/world-model-architecture.md`](specs/world-model-architecture.md) is the canonical framing. Read it before touching treatment, citation-graph, or synthesis code in either implementation repo.

Key sections, in priority order:

1. **The Four Layers** — Capabilities → World Model → Intelligence → Interfaces. The mental model the rest of the system is shaped against.
2. **Abstention as a First-Class Output** — binding principle on every capability, interface, and proactive signal. Four concrete rules; the fourth (*Validate against deployment distribution*) is the most recently added and has a worked-example link to the May 2026 text-pattern loader audit.
3. **How We Plan on Using It** — engineering posture. Names the user-side world model as the architectural top priority and the largest unstarted investment.

## What's not in this repo

Code, schemas, runbooks, operational state. This repo is doctrine — when implementation drifts from doctrine, fix the implementation or update the spec, but don't let the drift live silently.

## Rendering

`index.html` is a self-contained static renderer for the spec. The markdown is canonical; the HTML is a viewer.
