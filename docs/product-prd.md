# Bridge — Product Requirements (PRD)

> Canonical brief: [`../skill.md`](../skill.md). This document expands the product problem, jobs, surfaces, and decisions for the product owner.

## 1. Problem statement

People do not struggle only with "too many files." They struggle with **fragmented context**. Understanding is split across PDFs, slide decks, audio notes, recorded calls, webpages, spreadsheets, design files, personal notes, collaborator comments, AI conversations, and task tools.

Existing products solve slices:

- **NotebookLM** grounds answers in selected sources and generates audio/video overviews — but is notebook-bound, not spatial or work-aware.
- **Obsidian** enables spatial, relational thinking — but is not source-grounded or generative.
- **Jira** tracks work items and dependencies — but is detached from the evidence that created the work.
- **Figma** gives teams a low-friction shared file and canvas — but for design, not understanding.

None of them continuously converts raw, changing context into a private and shared understanding layer, then into explainers, documents, and related work. That conversion is Bridge.

## 2. Jobs to be done

Bridge solves five connected jobs:

1. **Collect & connect** heterogeneous sources (uploads + live connections).
2. **Understand** what matters (generated Map, grounded answers, citations).
3. **Work privately** before sharing meaning (private-by-default Workspace).
4. **Generate outputs** from the understanding (audio, video, documents, briefs).
5. **Coordinate action** without losing linkage to evidence (Context Work).

A sixth cross-cutting job: **bridge private thinking and shared meaning.** Users need a place to be uncertain, rough, wrong, and exploratory without polluting the canonical shared context.

## 3. Surfaces and requirements

### 3.1 Map (default, simplest)

- Generated, clean, adaptive; the user receives value before understanding the system.
- Shows: connected sources (top), understanding section (findings, questions, decisions, risks), contextual work signal nearby — never buried.
- **Adaptive templates**: decision map, research synthesis, handoff map, work map — chosen per Bridge, not one rigid artifact type.
- Absorbed "visual map" and "quick brief" — they served the same purpose: a compact explanation of the whole.
- Map is *not* for deep source reading.

### 3.2 Workspace (deepest)

- Infinite spatial canvas: source nodes, note nodes, question nodes, AI session nodes, generated output nodes, context frames.
- Not a reader, not a source tab, not a single-document screen.
- The canvas is for **arrangement and generation**: generated outputs land on the board as first-class, inspectable, source-linked cards.
- Private by default; explicit promotion to "shared with person," "shared with group," or "main context."

### 3.3 Context Work

- Work exists because something in the context needs follow-up.
- Every item answers: what needs doing / who owns it / what evidence created it / who can see it.
- Items originate from: a source, a map card, an open question, a changed file, a generated output, or a comment.
- Supports blockers and dependency links ("blocks" / "is blocked by"), owners, statuses — kept light, wrapped *around* Map and Workspace, never above them.

### 3.4 Activity (not a mode)

- Compact history of meaningful events: source connected, source changed, changes analyzed, note added, output generated, task created, context promoted.
- Timeline as a full mode was deliberately dropped; it may return only as a generated map template.

## 4. Live sources and the change gate

Bridge supports live connected sources (Google Docs/Slides/Sheets, Word/PowerPoint/Excel, Figma files, links, media) alongside local uploads.

**The most important product constraint:** live changes must never silently rewrite shared truth.

Flow when a connected file changes:

1. **Detect** the change (webhook/notification or reconciliation).
2. **Summarize** what changed.
3. **Assess** which map cards or tasks may be affected.
4. **Ask** the user whether to promote the change into main context.

Source states: `Live`, `Synced`, `Changed`, `In progress`, `Needs analysis`, `Analyzing`, `Added to context`, `Not in context`, `Private`, `Shared`.

## 5. Sharing model

Visibility is an attribute of every relevant object:

| Tier | Meaning |
|---|---|
| Private | Only the author; the Workspace default |
| Shared with person | Appears in that person's visible layer only |
| Shared with group | Scoped to a named group |
| Main context | Part of the canonical shared Bridge |

Sharing a note with one person does **not** add it to main context. Comments, notes, tasks, and outputs inherit or explicitly override visibility.

## 6. Generated outputs

Types: audio explainer, video explainer, combined document, visual summary, question list, study/working notes, handoff brief, interactive deep dive session.

Scopes: one source · multiple sources · a selected frame · a question cluster · the whole Bridge.

Requirements: outputs stay on-canvas, stay source-linked, remain inspectable and revisable. Interactive audio can become a conversational interface, not a dead artifact.

## 7. Standing decisions

1. Blank, commercial file start — no questionnaire, no wizard.
2. Map stays the simplest generated layer.
3. Workspace is a canvas + creation studio, never a "viewer with side panel."
4. Tasks and people stay contextual — no PM dashboard.
5. Chat stays assistive (Bridge Studio, selected objects, audio sessions) — never the primary surface.
6. Timeline demoted to Activity, intentionally.
7. Private / selectively-shared / main-context layers are strictly separated.

## 8. Non-goals

Not a chatbot-first app; not a project-management dashboard; not a one-source-at-a-time reader; not a messy knowledge graph; not an auto-merging truth machine; not a rigid workflow with mandatory setup.

## 9. Definition of success

A user can open a blank file, add messy context from many places, get a trustworthy Map quickly, go deep privately in Workspace, generate useful explainers or documents, and coordinate action through Context Work — while always being able to jump from any insight or output back to the original evidence.
