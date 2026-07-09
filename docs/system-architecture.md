# Bridge — System Architecture

> Canonical brief: [`../skill.md`](../skill.md). This document guides engineering.

Bridge requires a real architecture, not a frontend prototype. The single most important architectural idea: **Bridge is model-provider-agnostic.** The provider layer is a routing system, not a hard-coded dependency.

## The five subsystems

```
┌─────────────────────────────────────────────────────────┐
│  1. Native Mac client (SwiftUI)                          │
│     Map · Workspace canvas · Context Work · Activity     │
└──────────────┬──────────────────────────────────────────┘
               │
┌──────────────┴──────────────────────────────────────────┐
│  Backend services                                        │
│  ┌────────────────────┐  ┌───────────────────────────┐  │
│  │ 2. Source & Sync   │→ │ 3. Context Engine         │  │
│  │ connectors, OAuth, │  │ parse, chunk, embed,      │  │
│  │ change detection,  │  │ index, summarize,         │  │
│  │ webhooks, revisions│  │ compare, cite             │  │
│  └────────────────────┘  └───────────────────────────┘  │
│  ┌────────────────────┐  ┌───────────────────────────┐  │
│  │ 4. Workspace State │  │ 5. Generation &           │  │
│  │ canvas documents,  │  │    Orchestration          │  │
│  │ nodes, frames,     │  │ audio/video explainers,   │  │
│  │ visibility, versions│ │ docs, sessions, jobs,     │  │
│  └────────────────────┘  │ provider routing          │  │
│                          └───────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
```

### 1. Native Mac client

- **SwiftUI** as the UI layer (Apple's cross-platform framework for its platforms; pairs naturally with the Foundation Models framework, which is native Swift).
- **Apple Foundation Models framework** for on-device and Private Cloud Compute model access, and as an abstraction over other providers via the Language Model protocol.
- **App Sandbox** and macOS-safe file access must shape the app from the beginning if Mac App Store distribution is a goal — not retrofitted. File Provider is available later if Bridge needs deeper managed-cloud-file integration.

### 2. Source & sync backend

Owns: connectors, OAuth, file metadata, change detection, revisions, webhook receipts, and "analyze changes" jobs.

- Normalizes Google Drive, Microsoft 365, Figma, local uploads, and pasted links into a **single Bridge source model**.
- Prefers webhooks/change-notifications over polling (Google Drive push notifications, Microsoft Graph change notifications, Figma webhooks, Jira webhooks) — but **always maintains fallback reconciliation** for reliability.
- Details per provider: [`connector-specs.md`](connector-specs.md).

### 3. Context engine

Where parse → chunk → embed → index → summarize → compare → cite happens.

- Option A: provider-managed retrieval (e.g. OpenAI vector stores + file search automatically parse/chunk/embed/index uploads; Gemini exposes file-search surfaces too).
- Option B: own vector database.
- Either way, the same conceptual objects exist: **source, chunk, citation, evidence link, derived insight, update diff.**
- Diff-aware reanalysis: when a source changes, the engine produces an update diff and impact assessment (affected map cards, tasks) — feeding the human promotion gate.

### 4. Workspace state system

Essential and easy to underestimate. Workspace is a Figma-like board, not a static page.

- First-class storage for: nodes, frames, positions, dimensions, z-indexes, selections, visibility layers, and versions.
- Treat the Workspace as a **serializable canvas document** (Obsidian's open JSON Canvas format is instructive precedent), not a pile of view state.
- Visibility tiers (private / person / group / main context) are stored per object and enforced server-side.

### 5. Generation & orchestration layer

- Job orchestration for: audio explainers, video explainers, interactive sessions, combined documents, work-analysis suggestions.
- **Provider routing** across: local/on-device (Apple), OpenAI (Responses API: stateful interactions, multimodal, built-in tools, function calling, file search), Anthropic (Messages: multi-turn, streaming, tool use, vision, structured outputs), Gemini (interactions, real-time/voice models, file search), plus optional enterprise adapters.
- **Provenance is a hard requirement:** every generated output and every answer keeps recoverable links to the exact page, timestamp, note, or source range it came from.
- Generation jobs are async and must be reliable (queued, resumable, observable) — identified as one of the hardest engineering problems alongside source normalization, revision/change analysis, canvas persistence, and visibility layers.

## Backend module checklist (minimum system)

- [ ] Authentication and team/workspace access control
- [ ] OAuth and connectors (Google, Microsoft, Figma, …)
- [ ] File ingestion and parsing
- [ ] Change detection and source analysis
- [ ] Vector/retrieval and citation services
- [ ] Workspace canvas state storage
- [ ] Generated output orchestration
- [ ] Task/comment/activity services
- [ ] Visibility and context-promotion rules
- [ ] Audit/history and sharing controls

## Hardest problems (engineering focus)

1. **Source normalization** — one model over wildly different providers.
2. **Revision/change analysis** — diffs with meaning, not byte deltas.
3. **Provenance** — citation links that survive regeneration and edits.
4. **Canvas-state persistence** — multiplayer-ready, versioned, visibility-aware.
5. **Visibility layers** — enforced consistently across every object type.
6. **Reliable async generation** — long-running multimodal jobs with progress and failure recovery.
