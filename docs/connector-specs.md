# Bridge — Connector Specifications

> Canonical brief: [`../skill.md`](../skill.md). This document specifies the live-source connectors and the change-detection flow they share.

## Shared connector contract

Every connector normalizes its provider into the single Bridge source model. Each connector must supply:

1. **Auth** — OAuth flow, token storage, refresh, revocation handling.
2. **Metadata** — provider, owner, collaborators, last-updated, permalink.
3. **Content access** — fetch/export content in a parseable form for the context engine.
4. **Change detection** — webhook/push subscription where available.
5. **Revisions** — enumerate versions so Bridge can build meaningful diffs.
6. **Reconciliation** — periodic fallback sync for missed events (webhooks are never trusted as the only signal).

## The change gate (applies to every connector)

```
provider event ──→ Source status: Changed
                        │
                        ▼
                 summarize what changed (context engine, diff-aware)
                        │
                        ▼
                 assess impact: affected map cards, tasks
                        │
                        ▼
                 ASK THE USER ──→ promote to main context
                              └─→ keep as source only
```

No connector may write into main context directly. Detection and analysis are automatic; **promotion is always human-gated.**

## Google Workspace (Drive / Docs / Sheets / Slides)

- **Change detection:** Drive *changes* feed and *push notifications* (webhooks) for resource changes.
- **Revisions:** Drive *revisions* resource preserves modification history — the raw material for "what changed since last analysis."
- **Content:** Drive export endpoints for Docs/Sheets/Slides into parseable formats.
- **Notes:** revisions + changes + push notifications make the `Live → Changed → Needs analysis → …` lifecycle directly implementable.

## Microsoft 365 (Word / PowerPoint / Excel)

- **Change detection:** Microsoft Graph *change notifications* delivered to webhook endpoints; designed so client apps update their own views after resource changes.
- **Content:** Graph drive-item content/download endpoints.
- **Notes:** Bridge adds the missing human gate on top of Graph's notification model — Graph tells us *that* something changed; Bridge decides *whether it becomes shared truth.*

## Figma

- **API surface:** REST API exposes files, comments, file versions, users, projects, and activity logs.
- **Change detection:** Figma webhooks.
- **Content:** file structure and rendered exports; comments ingest as discussion context.
- **Notes:** Figma files are both a source type *and* the UX inspiration — a Figma source node on the Bridge canvas should feel native.

## Jira

- **Model mapping:** Jira work items (structured units with statuses, priorities, subtasks, security, links) map to/inform Bridge Context Work items; work-item links encode dependencies ("blocks" / "is blocked by") → Bridge `Blocker` edges.
- **Change detection:** Jira webhooks.
- **Notes:** Jira's security schemes (project access ≠ item visibility) validate Bridge's per-object visibility design. Bridge stays lighter than Jira — no board-first UX.

## Local uploads

- Direct ingestion: PDFs, decks, spreadsheets, media, audio notes.
- Status starts at `Synced` (no live change feed); revisions created on re-upload.

## Links & web pages

- Pasted URLs become sources with fetched/parsed content.
- Optional re-fetch for change detection (reconciliation-only; no webhooks).

## Media (audio/video)

- Uploaded or linked recordings; transcription feeds the context engine; timestamps become citation targets for evidence links.

## Connector build order (recommended)

1. **Local uploads** — no OAuth, unblocks the whole pipeline (parse → chunk → map → outputs).
2. **Google Drive** — richest revision/notification primitives; validates the change gate end-to-end.
3. **Figma** — high-value for design teams; webhooks + versions.
4. **Microsoft Graph** — enterprise breadth.
5. **Jira** — bidirectional Context Work sync (read first, write later).
