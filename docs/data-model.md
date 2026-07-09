# Bridge — Data Model

> Canonical brief: [`../skill.md`](../skill.md). This document defines the core entities and their rules.

## Entity overview

```
User ─── membership ──→ Workspace ──→ Bridge (the "file")
                                        │
        ┌───────────────┬───────────────┼────────────────┬─────────────┐
        ▼               ▼               ▼                ▼             ▼
     Source         MapCard      WorkspaceNode        Task      ActivityEvent
        │               │        WorkspaceFrame         │
        ├─ SourceRevision                               ├─ Blocker
        ├─ SourceChunk ──→ EvidenceLink ←── GeneratedOutput / AISession / Note / Comment
        └─ (provider metadata)
```

## Core entities

| Entity | Purpose | Key fields / notes |
|---|---|---|
| **User** | A person | identity, provider accounts |
| **Workspace** | Team/tenant container | members, access control |
| **Bridge** | The file a user opens | title, map template, main context |
| **Source** | Uploaded or live-connected input | provider, owner, collaborators, last_updated, last_analyzed, status, visibility |
| **SourceRevision** | A detected version of a source | diff summary, detected_at, promoted?(bool) |
| **SourceChunk** | Retrieval unit | parsed segment, embedding ref, location (page/timestamp/range) |
| **EvidenceLink** | Provenance edge | from any derived object → exact chunk/page/timestamp/range |
| **MapCard** | Generated understanding unit | kind (finding/question/decision/risk/next-action), evidence links, visibility |
| **WorkspaceNode** | Item on the canvas | type (source/note/question/ai-session/output), position, dimensions, z-index, visibility |
| **WorkspaceFrame** | Named canvas grouping | children, position, visibility; usable as a generation scope |
| **Note** | User-authored thought | body, tags, tagged people, visibility |
| **Comment** | Discussion on an object | target, author, visibility (inherits or overrides) |
| **GeneratedOutput** | Audio/video/document/etc. | type, generation scope, job status, evidence links, visibility |
| **AISession** | A conversation node | provider, transcript, grounding set, visibility |
| **Task** | Context Work item | title, owner, status, priority, origin links (required), visibility |
| **Blocker** | Dependency edge | task ↔ task: "blocks" / "is blocked by" |
| **PeopleAccessScope** | Sharing grant | object → person/group tier |
| **ActivityEvent** | History record | kind, actor, object refs, timestamp |
| **Permission** | Access rule | workspace/bridge/object level |

## Source status lifecycle

```
                    ┌── local upload ──→ Synced
   connect/upload ──┤
                    └── live source ───→ Live
                                          │  provider change detected
                                          ▼
                                       Changed ──→ Needs analysis ──→ Analyzing
                                                                        │
                                             ┌──────────────────────────┤
                                             ▼                          ▼
                                    Added to context            Not in context
                                    (human approved)         (keep as source only)
```

Full status set: `Live`, `Synced`, `Changed`, `In progress`, `Needs analysis`, `Analyzing`, `Added to context`, `Not in context`, `Private`, `Shared`.

**Invariant:** a `SourceRevision` never reaches "Added to context" without an explicit human promotion.

## Visibility model

Every relevant object (source, node, note, comment, task, output, map card) carries a visibility value:

1. `private` — author only; the Workspace default
2. `person:<id>` — shared with selected person(s)
3. `group:<id>` — shared with a group
4. `main` — promoted to main Bridge context

Rules:

- Child objects (comments on a note, tasks from a card) **inherit** visibility unless explicitly overridden.
- A note tagged to a person enters that person's visible layer — never `main` automatically.
- Promotion to `main` is an explicit event, recorded as an `ActivityEvent`.

## Task invariants (Context Work)

A `Task` must link to at least one origin: source, map card, question, decision, risk, workspace note, changed external file (`SourceRevision`), generated output, comment, or unresolved context gap.

Every task answers: **what** (title/description), **who** (owner), **why/evidence** (origin links), **who can see it** (visibility).

## Provenance invariant

Every `MapCard`, `GeneratedOutput`, and grounded `AISession` answer must hold `EvidenceLink`s that resolve to an exact location — page, timestamp, note, or source range. Provenance survives regeneration: regenerating an output produces a new version with fresh links, never orphaned claims.

## Canvas document

The Workspace serializes as a canvas document (nodes + frames + layout + visibility + version), in the spirit of the open JSON Canvas format — a real document, not ephemeral view state. Versioned to support history and future multiplayer.
