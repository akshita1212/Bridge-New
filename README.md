# Bridge

**Bridge is a source-grounded context system.** It helps a person or a team turn scattered inputs — PDFs, live Google/Microsoft/Figma files, notes, recordings, links, AI conversations — into usable understanding, and then turn that understanding into outputs (audio/video explainers, documents) and coordinated action.

Bridge is inspired by NotebookLM, Obsidian, Figma, and Jira, but it is not a clone of any of them:

| Inspiration | What Bridge borrows |
|---|---|
| **NotebookLM** | Source-grounded understanding; generated audio & video overviews; interactive sessions |
| **Obsidian** | Infinite-canvas spatial thinking; note-linking; private-first knowledge work |
| **Figma** | File-based entry; collaborative canvas; "start simple, go deep later" |
| **Jira** | Structured work items with owners, status, and dependency links |

## Core product promise

> Drop anything. Bridge makes it understandable.
> Go deeper only when you want.
> Keep the source visible.
> Never add changing external content into shared truth without approval.

## The three functional layers (+ Activity)

1. **Map** — the simplest, generated view of understanding: connected sources, key findings, open questions, decisions, risks, and light work signals. Adaptive per project (decision map, research synthesis, handoff map…).
2. **Workspace** — the deep, private-by-default spatial canvas where sources, notes, questions, AI sessions, and generated outputs coexist as nodes on one board. Not a reader — a context canvas and creation studio.
3. **Context Work** — the contextual action layer. Every task answers: *what needs doing, who owns it, and what evidence created it.* Work stays linked to sources, never floats free.

**Activity** is a compact change/history layer (source changed, output generated, task created, context promoted) — deliberately *not* a main mode.

## Repository documentation

| File | Purpose | Owner focus |
|---|---|---|
| [`skill.md`](skill.md) | Canonical product brief — the single source of truth | Everyone |
| [`docs/product-prd.md`](docs/product-prd.md) | Problem, jobs-to-be-done, surfaces, decisions, non-goals, success definition | Product |
| [`docs/ux-principles.md`](docs/ux-principles.md) | Progressive disclosure, blank start, visibility layers, calm surface | UX / Design |
| [`docs/system-architecture.md`](docs/system-architecture.md) | Five subsystems: Mac client, source & sync backend, context engine, workspace state, generation orchestration | Engineering |
| [`docs/data-model.md`](docs/data-model.md) | Core entities, statuses, visibility model, provenance | Engineering / AI |
| [`docs/connector-specs.md`](docs/connector-specs.md) | Google Drive, Microsoft Graph, Figma, Jira connectors and change-detection flows | Integrations |

## Defining constraints (decisions, not vibes)

- Bridge starts as a **blank, commercial file** — no setup wizard, no questionnaire.
- **Map stays simple**; deep reading and creation live in Workspace.
- **Workspace is a canvas, not a document viewer** with a side panel.
- **Tasks are contextual**, not a project-management dashboard.
- **Chat is assistive, never dominant** — the surface is the file, the map, the workspace.
- **Changed sources never silently rewrite shared truth** — detect → analyze → human-gated promotion to main context.
- **Three visibility tiers everywhere**: private workspace → shared with person/group → main Bridge context.
