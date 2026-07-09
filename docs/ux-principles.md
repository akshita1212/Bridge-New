# Bridge — UX Principles

> Canonical brief: [`../skill.md`](../skill.md). This document guides UX and design-system work.

## 1. Progressive disclosure is the core UX strategy

The interaction arc every Bridge follows:

1. **Blank start** — a new Bridge opens like a commercial blank file (Figma-style), not a dashboard or questionnaire. Approachable, calm, visually generous.
2. **Generated Map** — as the user adds context, Bridge generates understanding. The user receives value before learning the system.
3. **Deep spatial work** — Workspace appears as depth the user opts into, not a wall they must climb.
4. **Contextual actions** — right-click menus and drawers on objects, not global chrome. Advanced power stays underneath a calm surface.

Never overwhelm the user on entry. Too much UI too early weakens the core promise ("drop anything, Bridge makes it understandable").

## 2. The surface hierarchy

- **Map** is the inside space and the first thing a user sees once context exists. It must feel *generated, clean, adaptive*: connected sources at the top, understanding beneath, work signals nearby — never buried in another tab.
- **Workspace** is an infinite canvas in the Obsidian Canvas / Figma tradition: nodes, frames, free spatial arrangement, grouping by proximity and frames. It is a thinking and creation space — if it starts to look like a document viewer with a side panel, the design has failed.
- **Context Work** renders *around* Map and Workspace (badges, drawers, linked panels), never as a board-first main tab.
- **Activity** is a compact, dismissible history layer — not a page the user must maintain.

## 3. Visibility must be visible

The three-tier model (private → shared with person/group → main context) must be legible on every object at a glance:

- Every node, note, task, comment, and output shows its visibility state.
- Promotion to main context is an explicit, deliberate act — a ceremony, not a default.
- A user must always be able to answer "who can see this?" without opening a settings pane.
- Private work must *feel* private: users need a place to be uncertain, anxious, rough, wrong, exploratory, and incomplete without polluting shared truth.

## 4. Changed sources ask; they never act

When a live source changes, the UX presents: what changed → what it may affect (map cards, tasks) → an explicit choice to add to main context or keep as source-only. Silent auto-merge is forbidden. Status chips (`Changed`, `Needs analysis`, `Analyzing`, `Added to context`…) carry this state.

## 5. Chat is assistive, never dominant

Chat surfaces exist in Bridge Studio, on selected objects, in interactive audio sessions, and inside generated explanations. The product's face is the file, the map, and the workspace — never a chat box as the home screen.

## 6. Outputs are objects, not downloads

Generated audio/video/document outputs render as first-class cards on the canvas: playable, inspectable, revisable, and visibly linked to their sources. Provenance links (back to page, timestamp, note, or source range) are part of the UI, not metadata trivia.

## 7. Calm, commercial polish

Bridge must remain visually calm and commercially polished even while handling live documents, notes, tasks, questions, audio and video explainers, shared context, and private layers. The design-system requirement: preserve a calm, Figma-like surface even as advanced capabilities appear underneath.

## 8. Anti-patterns (design smells)

- A setup wizard or mandatory template chooser on first open.
- A global chat box as the primary entry point.
- A kanban/board landing page.
- A PDF reader occupying the whole screen with tools in a sidebar.
- Any flow where an external file change appears in shared context without a human decision.
- Visibility states hidden behind menus.
