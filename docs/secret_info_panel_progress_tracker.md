# Secret Info Panel Progress Tracker

## Purpose

Use this document to track implementation progress across the current roadmap workstreams and to hand work forward without losing architectural decisions.

This tracker is aligned to the live `1.5.0.5` baseline, the current architecture document, and the current roadmap on `main`.

## Current Project State

- Project: Secret Info Panel
- Roadmap Reference: `docs/secret_info_panel_roadmap.md`
- Architecture Reference: `docs/secret_info_panel_architecture.md`
- Current Working Version: `1.5.0.5`
- Current Focus: groundwork packet before live routing
- Last Updated: `2026-03-27`

### Baseline State Confirmed in `1.5.0.5`

- Sidebar-first control surface remains active.
- Prompt fields, current output, generated state, prompt preview, and routed state are persisted separately during active use.
- Current output now includes canonical body and derived display text layers.
- Raw diagnostic response and raw response body are preserved separately.
- Same-turn reroll seed protection remains intact.
- Secret-info editing is body-only at the canonical edit layer.
- tempStorage is used for ephemeral edit-session drafts and active-field state.
- Routed-state scaffolding exists for `memory`, `authorsNote`, and `lorebook`.
- Diagnostics expose route adapter and route receipt summaries.
- The script intentionally performs a clean-slate initialization reset on script load.

### Immediate Interpretation

`1.5.0.5` is beyond the older pre-contract baseline. Canonical-output handling and routed-state scaffolding are implemented baseline, not future concepts.

The next work is **not** direct production routing.

The current groundwork packet is:

- explicit authority and edit-ownership cleanup
- script-panel migration for dense working/editing surfaces
- preview-only routing planner extension on top of the routed-state scaffold
- small injection-ready config tightening needed for later routing

Live apply / clear / resync into Memory, Author's Note, and Lorebook remains deferred until that groundwork is stable.

### Initialization Interpretation

The current live script intentionally resets its own panel state to a fresh baseline on initialization.

That should be treated as intended behavior, not as a persistence bug.

Interpret current persistence this way:

- active-use state boundaries matter
- canonical ownership still matters
- diagnostics and route scaffolding still matter
- but the script does **not** currently target preserving panel state unchanged through initialization
- future work should preserve the clean-slate initialize behavior unless project direction changes explicitly later

## Workstream Status Board

| Workstream | Name | Status | Live Baseline | Notes |
|---|---|---|---|---|
| A | Commit Semantics and Authority Cleanup | Not Started | `1.5.0.5` baseline exists | Canonical body, derived display text, raw diagnostics, and temp draft infrastructure exist, but explicit ownership/commit semantics still need tightening. |
| B | Dense Editor UI Surface Migration | Not Started | sidebar-first baseline | Dense editing still lives in the sidebar today; the script panel migration has not started. |
| C | Routing-Ready Preview Planner | Not Started | scaffold baseline exists | Routed-state scaffolding exists, but preview-only target planning from committed canonical body is not implemented yet. |
| D | Injection-Ready Config Tightening | Not Started | partial baseline only | Config-like values exist, but the small roadmap-aligned config layer for later routing is not yet tightened. |
| E | Live Routing and Splicing Engine | Deferred | scaffold only | Do not start live apply / clear / resync until workstreams A-D are stable. |
| F | Lorebook and Source Groundwork | Deferred | no dedicated implementation yet | This should land closer to prompt/routing co-development, not at the start of the groundwork packet. |
| G | Prompt and Limited Routing Co-Development | Deferred | stable soft-style baseline | Prompt expansion and limited live routing depend on the groundwork packet landing first. |
| H | Diagnostics, Sync, and Repair Completion | In Progress | current diagnostics baseline | Diagnostics already expose generation and route-scaffold state, but target sync validation and repair flows are not complete. |

Status values: Not Started, In Progress, Blocked, Complete, Deferred

## Locked Decisions

- The live script is the implementation source of truth.
- The roadmap on `main` is the future-direction source of truth for current sequencing.
- Canonical body is the downstream source for future routing.
- Raw model output remains diagnostic only.
- Derived display text is rebuilt from canonical body rather than treated as the editable truth.
- Unsaved draft state must remain draft-only.
- Sidebar remains the compact control and monitoring surface.
- The script panel is the intended dense working/editing surface.
- Diagnostics remain last in the sidebar.
- tempStorage is valid for ephemeral editor/session convenience only.
- Same-turn rerolls must remain anchored to a stable seed for the current story turn.
- Manual refresh remains the safe baseline.
- Auto-refresh remains opt-in only.
- Narrow structural changes remain preferred over broad rewrites.
- Routed-state scaffolding already exists and should be extended, not discarded.
- Live routing must derive from committed canonical body, not raw diagnostic output.
- Clean-slate initialization behavior on script load is intended baseline behavior and should be preserved unless project direction changes explicitly later.

## Open Decisions

- Exact save/cancel flow wording and commit UX for dense editors
- Preview-planner state shape for dirty/current/planned target status
- Small config layer shape for output mode, target enablement, route behavior, and editor-surface preferences
- Lorebook routing model: one master entry, per-character entries, or hybrid
- Manual edit sync behavior once live routing exists: auto-apply, dirty-state only, or explicit apply
- Route apply behavior on refresh
- Receipt shape: hash-only, version counter, or richer per-target receipt object
- Ownership marker format and replacement policy for each target surface
- Default multi-character output mode
- Author's Note scope and length budget

## Workstream Detail Logs

### A - Commit Semantics and Authority Cleanup

**Status:** Not Started

Completion target:

- explicit save/cancel semantics for user edits
- unsaved edits remain draft-only
- raw diagnostic layers remain diagnostic and fallback only
- canonical body changes only on explicit commit
- derived display text rebuilds from committed canonical body

This is the highest-priority code-facing workstream, but documentation and sequencing should be aligned before code work starts.

### B - Dense Editor UI Surface Migration

**Status:** Not Started

Completion target:

- sidebar remains for refresh controls, summaries, route state, and diagnostics
- script panel becomes the main dense working/editing surface
- canonical body editor, prompt editors, previews, and save/cancel actions move out of the sidebar
- no long-lived confusion remains between competing editor surfaces

### C - Routing-Ready Preview Planner

**Status:** Not Started

Completion target:

- extend the routed-state scaffold into preview-only planning
- compute per-target desired fragments from committed canonical body
- represent dirty/current/planned target state clearly
- expose target previews for `memory`, always-on Lorebook, and `authorsNote`
- keep this layer preview-only until live routing is intentionally started later

### D - Injection-Ready Config Tightening

**Status:** Not Started

Completion target:

- formalize the small set of config decisions needed for later routing
- keep the mapping tight rather than introducing a broad schema framework
- support future output mode, target enablement, route behavior, and editor-surface preferences
- avoid over-abstracted config infrastructure

### E - Live Routing and Splicing Engine

**Status:** Deferred

Completion target when this workstream opens:

- preview, apply, clear, and resync flows exist
- the script updates only its owned blocks
- per-target sync state reflects real applied content
- receipt validation works against live target state

This workstream should not begin until A-D have produced stable, testable contracts.

### F - Lorebook and Source Groundwork

**Status:** Deferred

Completion target when this workstream opens:

- hybrid source model support exists
- always-on Lorebook can be represented as a future lane in planning state
- character/source choices remain understandable and testable

This belongs shortly before the main prompt-engineering and live-routing phase, not at the start of the current packet.

### G - Prompt and Limited Routing Co-Development

**Status:** Deferred

Completion target when this workstream opens:

- prompt engineering is developed alongside carefully limited live routing
- preservation of unrelated user-authored content remains testable
- manual baseline and safe defaults remain intact

### H - Diagnostics, Sync, and Repair Completion

**Status:** In Progress

Already present:

- current diagnostics cover run state, prompt checks, current output, raw response, canonical contract version, route adapter summary, and route receipt summary

Still missing:

- target-specific applied-content inspection
- real sync-state validation against live targets
- repair / reapply actions
- route ownership troubleshooting flows

## Regression Checklist

Run this before marking later workstreams complete if they touch shared behavior.

- same-turn reroll protection still works
- prompt preview still rebuilds correctly
- canonical body persists correctly during active use
- display text still rebuilds correctly from canonical body
- raw diagnostics remain preserved
- manual edits do not mutate raw diagnostics
- temp edit-session state does not become a durable source of truth
- route scaffolding fields remain coherent after save/load cycles during active use
- preview-planner state remains preview-only until live routing is intentionally introduced
- diagnostics remain last in the sidebar
- no live story-context contamination is introduced early
- intended clean-slate initialization behavior remains unchanged

## Next Handoff Block

```md
Project: Secret Info Panel
Architecture Reference: docs/secret_info_panel_architecture.md
Roadmap Reference: docs/secret_info_panel_roadmap.md
Progress Tracker: docs/secret_info_panel_progress_tracker.md
Current Working Version: 1.5.0.5
Current Focus: groundwork packet before live routing
Primary Active Workstreams: A commit semantics and authority cleanup; B dense editor UI migration; C routing-ready preview planner; D injection-ready config tightening
Completed Baseline: canonical output contract landed; routed-state and receipt scaffolding landed; diagnostics baseline landed; sidebar-first operating surface still active; clean-slate initialize behavior is intentional baseline behavior
In-Progress Workstreams: H diagnostics baseline only
Deferred Until After Groundwork: E live routing and splicing engine; F lorebook and source groundwork; G prompt and limited routing co-development
Locked Decisions: live script is source of truth; roadmap on main defines current sequencing; canonical body routes downstream; raw output remains diagnostic; derived display text is rebuilt; unsaved drafts stay draft-only; sidebar stays compact control surface; script panel becomes dense working surface; diagnostics remain last; tempStorage drafts are ephemeral only; reroll seed protection must remain intact; manual refresh remains safe baseline; auto-refresh remains opt-in; clean-slate initialization behavior is intended and preserved unless direction changes explicitly
Open Decisions Still Pending: dense-editor commit UX; preview-planner state shape; small config layer shape; lorebook routing model; manual edit sync behavior; receipt shape; route apply behavior on refresh; ownership marker policy; default multi-character mode; Author's Note scope
Known Risks / Bugs: avoid letting draft state become silent durable truth during active use; avoid collapsing canonical/display/raw separation; avoid jumping into live routing before the groundwork packet stabilizes; avoid breaking reroll protection; avoid broad rewrites that discard scaffolded route state; avoid accidentally weakening intended initialize behavior
What Must Be Preserved: `1.5.0.5` baseline contracts, canonical-body source-of-truth rule, reroll protection, diagnostics last, intended clean-slate initialization behavior, narrow structural changes only
Requested Task For This Chat:
```

## Alignment Note

This tracker should now be read as a roadmap-aligned sequencing document.

If documents disagree about what to build next:

1. `live/secret_info_panel_live.naiscript`
2. `live/current_version.md`
3. `docs/changelog.md`
4. `docs/secret_info_panel_roadmap.md`
5. `docs/secret_info_panel_architecture.md`
6. this tracker
