# Secret Info Panel Progress Tracker

## Purpose

Use this document to track implementation progress across the current roadmap workstreams and to hand work forward without losing architectural decisions.

This tracker is aligned to the live `1.5.0.5` baseline.

## Current Project State

- Project: Secret Info Panel
- Roadmap Reference: `docs/secret_info_panel_roadmap.md`
- Architecture Reference: `docs/secret_info_panel_architecture.md`
- Current Working Version: `1.5.0.5`
- Current Focus: production routing on top of the landed canonical-output contract
- Last Updated: `2026-03-27`

### Baseline State Confirmed in `1.5.0.5`

- Sidebar-first control surface remains active.
- Prompt fields, current output, generated state, prompt preview, and routed state are persisted separately.
- Current output now includes canonical body and derived display text layers.
- Raw diagnostic response and raw response body are preserved separately.
- Same-turn reroll seed protection remains intact.
- Secret-info editing is body-only at the canonical edit layer.
- tempStorage is used for ephemeral edit-session drafts and active-field state.
- Routed-state scaffolding exists for `memory`, `authorsNote`, and `lorebook`.
- Diagnostics expose route adapter and route receipt summaries.

### Immediate Interpretation

`1.5.0.5` is beyond the older `1.4.21` baseline. Canonical-output handling and routed-state scaffolding are implemented baseline, not future concepts. The unfinished work is production routing, later UI/editor migration, and deeper validation.

## Package Status Board

| Package | Name | Status | Live Baseline | Notes |
|---|---|---|---|---|
| P1 | Canonical Output Contract | Complete | `1.5.0.5` | Canonical body, derived display text, and preserved raw response layers are implemented. |
| P2 | Routed-State and Receipt Scaffolding | Complete | `1.5.0.5` | Target adapters, ownership markers, desired signatures, and receipt placeholder fields exist. |
| P3 | Production Routing and Splicing Engine | Not Started | scaffold only | No live apply / clear / resync into Memory, Author's Note, or Lorebook yet. |
| P4 | Config Registry and UI Wiring Completion | Not Started | partial baseline only | Config-like values exist, but there is no full resolved config registry. |
| P5 | Dense Editor UI Surface | Not Started | sidebar-only baseline | temp draft editing exists, but the larger editor-surface migration has not started. |
| P6 | Prompt and Multi-Character Quality Expansion | Not Started | stable soft-style baseline | Current prompts are stable, but broader quality and multi-character work is still ahead. |
| P7 | Diagnostics, Sync, and Repair Completion | In Progress | current diagnostics baseline | Diagnostics already cover generation and route-scaffold visibility, but target sync and repair flows are not complete. |

Status values: Not Started, In Progress, Blocked, Complete, Deferred

## Locked Decisions

- The live script is the implementation source of truth.
- Canonical body is the downstream source for future routing.
- Raw model output remains diagnostic only.
- Derived display text is rebuilt from canonical body rather than treated as the editable truth.
- Sidebar remains the active control and monitoring surface.
- Diagnostics remain last in the sidebar.
- tempStorage is valid for ephemeral editor/session convenience only.
- Same-turn rerolls must remain anchored to a stable seed for the current story turn.
- Narrow structural changes remain preferred over broad rewrites.
- Route scaffolding now exists and should be extended, not discarded.

## Open Decisions

- Lorebook routing model: one master entry, per-character entries, or hybrid
- Manual edit sync behavior once routing exists: auto-apply, dirty-state only, or explicit apply
- Route apply behavior on refresh
- Receipt shape: hash-only, version counter, or richer per-target receipt object
- Ownership marker format and replacement policy for each target surface
- Default multi-character output mode
- Author's Note scope and length budget
- How much of config completion should be schema-driven versus tightly mapped UI

## Package Detail Logs

### P1 - Canonical Output Contract

**Status:** Complete

Implemented baseline:

- `canonicalBody` is persisted
- `derivedDisplayText` is persisted
- `lastRawResponse` and `lastRawResponseBody` are preserved
- current-output storage is no longer a single-string mental model

Future work should extend this contract, not reopen it.

### P2 - Routed-State and Receipt Scaffolding

**Status:** Complete

Implemented baseline:

- `secret-info-panel-routed-state-v1` exists
- targets exist for `memory`, `authorsNote`, and `lorebook`
- adapter metadata, ownership marker fields, desired signatures, and receipt fields are present
- diagnostics expose route adapter and receipt summaries

The next step is target application, not re-laying route storage.

### P3 - Production Routing and Splicing Engine

**Status:** Not Started beyond scaffolding

Completion target:

- preview, apply, clear, and resync flows exist
- the script updates only its owned blocks
- per-target sync state reflects real applied content

Build outward from the current routed-state container and canonical body contract.

### P4 - Config Registry and UI Wiring Completion

**Status:** Not Started

Treat current config-like fields as partial baseline, not as completed config infrastructure.

### P5 - Dense Editor UI Surface

**Status:** Not Started

The tempStorage edit-session workflow is useful baseline infrastructure, but it is not the editor-surface migration.

### P6 - Prompt and Multi-Character Quality Expansion

**Status:** Not Started

Current prompt behavior is the stable baseline that later quality work should preserve unless intentionally revised.

### P7 - Diagnostics, Sync, and Repair Completion

**Status:** In Progress

Already present:

- current diagnostics cover run state, prompt checks, current output, raw response, canonical contract version, route adapter summary, and route receipt summary

Still missing:

- target-specific applied-content inspection
- real sync-state validation against live targets
- repair / reapply actions
- route ownership troubleshooting flows

## Regression Checklist

Run this before marking later packages complete if they touch shared behavior.

- same-turn reroll protection still works
- prompt preview still rebuilds correctly
- canonical body persists correctly
- display text still rebuilds correctly from canonical body
- raw diagnostics remain preserved
- manual edits do not mutate raw diagnostics
- temp edit-session state does not become a durable source of truth
- route scaffolding fields remain coherent after save/load cycles
- diagnostics remain last in the sidebar

## Next Handoff Block

```md
Project: Secret Info Panel
Architecture Reference: docs/secret_info_panel_architecture.md
Roadmap Reference: docs/secret_info_panel_roadmap.md
Progress Tracker: docs/secret_info_panel_progress_tracker.md
Current Working Version: 1.5.0.5
Current Focus: P3 - Production Routing and Splicing Engine
Completed Packages: P1 canonical output contract; P2 routed-state and receipt scaffolding
In-Progress Packages: P7 diagnostics/sync baseline only
Locked Decisions: live script is source of truth; canonical body routes downstream; raw output remains diagnostic; derived display text is rebuilt; diagnostics remain last; tempStorage drafts are ephemeral only; reroll seed protection must remain intact
Open Decisions Still Pending: lorebook routing model; manual edit sync behavior; receipt shape; route apply behavior on refresh; ownership marker policy; default multi-character mode; Author's Note scope
Known Risks / Bugs: avoid breaking reroll protection; avoid collapsing canonical/display/raw separation; avoid replacing scaffolded route state with a broad rewrite; avoid letting temp draft state become durable truth
What Must Be Preserved: sidebar-first monitoring surface, canonical-body source-of-truth rule, reroll protection, diagnostics last, narrow structural changes only
Requested Task For This Chat:
```
