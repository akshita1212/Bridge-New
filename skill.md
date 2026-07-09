# Bridge — Canonical Product Brief (skill.md)

This file is the canonical written form of the Bridge product knowledge. Detailed expansions live in `docs/`; when documents disagree, this file wins.

## Product identity

Bridge is a **source-grounded context system**.

It helps a person or team gather scattered material, understand it in a generated **Map**, work privately and deeply in **Workspace**, generate outputs such as audio/video/docs, and coordinate follow-through through a contextual work layer (**Context Work**).

Bridge is inspired by NotebookLM, Obsidian, Figma, and Jira, but it is not a clone of any of them. NotebookLM contributes source-grounded synthesis and media generation; Obsidian contributes spatial note relationships and private thinking space; Figma contributes the file-and-canvas mental model; Jira contributes structured work ownership and state.

The full internal definition:

> Bridge is an adaptive, source-grounded context workspace that helps users collect live and static inputs, understand them in a generated Map, explore and create in a spatial Workspace, and organize follow-through through Context Work.

Use this definition instead of looser phrases like "AI workspace" or "knowledge tool."

## Core product promise

- Drop anything. Bridge makes it understandable.
- Go deeper only when you want.
- Keep the source visible.
- Do not add changing external content into shared truth without approval.

## Primary surfaces

**Map** is the simple inside space. It shows connected sources, key findings, questions, decisions, risks, next actions, and light Context Work indicators. It is generated, clean, and adaptive — one Bridge may need a decision map, another a research synthesis, another a handoff or work map ("adaptive templates").

**Workspace** is the deep, spatial canvas. It contains source nodes, note nodes, question nodes, AI session nodes, generated output nodes, and context frames. It supports private thinking first, selective sharing second, and main-context promotion third. The canvas is not only for arrangement but also for **generation**: a workspace can contain the original PDF, a note cluster, a generated audio explainer card, a video explainer card, a combined document card, and an interactive session node — all on one board.

**Context Work** is the action layer around the understanding. Work items originate from context and stay linked to evidence, owners, blockers, and visibility.

**Activity** is not a main mode. It is a compact change/history layer driven by source changes, task changes, generated outputs, and context updates.

## Source model

A Bridge source can be a **local upload** or a **connected live source**. Live connected sources include Google Drive/Docs/Sheets/Slides, Microsoft files (Word/PowerPoint/Excel), Figma files, links, and media.

Each source carries metadata for: provider, owner, collaborators, last updated, last analyzed, status, and visibility.

Source statuses (minimum set): `Live`, `Synced`, `Changed`, `In progress`, `Needs analysis`, `Analyzing`, `Added to context`, `Not in context`, `Private`, `Shared`.

**Key rule:** changed sources are never automatically merged into shared understanding. They are first *detected*, then *analyzed* (what changed, which map cards/tasks are affected), then *optionally promoted* to main context by a human.

## Sharing and privacy model

- Workspace defaults to **private**.
- Every object can be marked: **Private** · **Shared with selected person** · **Shared with group** · **Added to main context**.
- Comments, notes, tasks, and outputs must inherit or explicitly override visibility.
- A note tagged to a person goes to that person's visible layer — not automatically into main context.

This layered visibility is the product's protection against **context pollution** and one of its defining principles.

## Generated outputs

Bridge should generate, store, and keep on-canvas:

- Audio explainer
- Video explainer
- Combined document
- Visual summary
- Question list
- Study/working notes
- Handoff brief
- Interactive deep dive session

Outputs can be generated from: one source, multiple sources, a selected frame, a question cluster, or the whole Bridge.

Generated outputs must remain **source-linked and inspectable** — first-class objects inside the same workspace, never dead artifacts.

## Context Work rules

Tasks do not exist on their own. A task in Bridge must be linked to one or more of: source, map card, question, decision, risk, workspace note, changed external file, generated output, comment, or unresolved context gap.

Every task must answer:

1. What needs to happen
2. Who owns it
3. What evidence or context created it
4. Whether it is private, selectively shared, or part of main context

Jira's work-item model is the structural inspiration, but Bridge stays lighter and more contextual than a board-first project tool.

## macOS app direction

Bridge is designed so a native Mac app is feasible from the beginning. **SwiftUI** is the default UI layer. Apple's **Foundation Models framework** is strategically important: on-device and Private Cloud Compute model access from Swift, plus abstraction over other providers through the Language Model protocol. **App Sandbox** and secure file access must shape the app from the start if Mac App Store distribution is a goal.

## AI provider strategy

Bridge uses a **provider abstraction layer** (a routing system), never a single hard-coded vendor. Users can bring their own providers or use internal defaults.

Recommended provider categories:

- **Local/on-device** (Apple Foundation Models) for privacy-sensitive or lightweight flows
- **OpenAI** for multimodal responses, retrieval/file-search workflows, tool-based orchestration
- **Anthropic** for long-form reasoning, messages, tool use, streaming
- **Gemini** for interactions and live/voice-first models, file search
- **Optional internal enterprise adapters** for teams with their own infrastructure

## Backend modules (minimum system)

- Authentication and team/workspace access control
- OAuth and connectors for Google, Microsoft, Figma, and other providers
- File ingestion and parsing
- Change detection and source analysis
- Vector/retrieval and citation services
- Workspace canvas state storage
- Generated output orchestration
- Task/comment/activity services
- Visibility and context-promotion rules
- Audit/history and sharing controls

## Core data objects

Users, workspaces, bridges, sources, source revisions, source chunks, evidence links, map cards, workspace nodes, workspace frames, notes, comments, generated outputs, AI sessions, tasks, blockers, people access scopes, activity events, and permissions.

## Role-based guidance

- **Product:** protect the simplicity of Map while making Workspace and Context Work powerful enough for real use.
- **UX:** progressive disclosure — blank start, generated map, deep spatial work, contextual right-click and drawer actions; never overwhelm on entry.
- **Engineering:** the hardest parts are source normalization, revision/change analysis, provenance, canvas-state persistence, visibility layers, and reliable async generation jobs.
- **AI engineering:** model routing, chunking/indexing, answer grounding, diff-aware reanalysis, and output-generation pipelines.
- **Design systems:** preserve a calm, Figma-like surface even as advanced capabilities appear underneath.

## Standing product decisions (keep / drop / don't confuse)

1. Bridge starts like a **commercial blank file**, not a questionnaire. Too much UI too early weakens the core promise.
2. **Map remains the simplest generated layer** — not a place for deep source reading.
3. **Workspace remains a context canvas and creation studio**, not a source reader. Rebuilding a "document viewer plus side panel" misses the product.
4. **Task and people systems remain contextual** — helping people act on understanding, not another PM suite.
5. **Chat remains assistive, not dominant.** It lives in Bridge Studio, on selected objects, in audio sessions, and in generated explanations — but the surface is the file, the map, and the workspace.
6. **Timeline as a full mode was dropped intentionally.** Activity survives. Timeline may reappear as a generated map template, never as a default major tab.
7. **Private workspace, shared-with-person context, and main Bridge context are strictly separated.** This is a defining principle, not an edge case.

## Non-goals

Bridge must not become:

- a chatbot-first app
- a classic project-management dashboard
- a one-source-at-a-time reader
- a messy knowledge graph
- an auto-merging truth machine that silently promotes source changes
- a rigid research workflow with mandatory setup wizards

## Definition of success

Bridge succeeds when a user can open a blank file, add messy context from many places, get a trustworthy Map quickly, go deep privately in Workspace, generate useful explainers or documents, and coordinate action through Context Work — all while preserving the ability to jump back from any insight or output to the original evidence.
