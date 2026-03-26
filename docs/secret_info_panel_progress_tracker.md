# Secret Info Panel Progress Tracker

## Purpose

Use this document to track implementation progress across roadmap packages and to hand work forward into a new chat without losing architectural decisions.

---

## Current Project State

- Project: Secret Info Panel
- Roadmap Reference: `SecretInfoPanel_Roadmap_v1_1.md`
- Current Working Version: `1.4.21`
- Current Focus Package: `P1 — Canonical Data Contract`
- Last Completed Package: `Baseline hardening pass through 1.4.21 (pre-package roadmap alignment)`
- Last Updated: `2026-03-25`

### Baseline State Confirmed in `1.4.21`

- Sidebar-first control surface remains the active operating model.
- Four-layer storage/workflow separation remains intact:
  - prompt fields
  - current output
  - generated/runtime state
  - prompt preview
- Same-turn reroll seed protection remains intact.
- Recent history remains limited in the sidebar.
- Diagnostics remain last in the sidebar.
- Secret-info output is now edited as body-only content.
- Lightweight leading-label normalization/sanitization exists at live contract boundaries.
- tempStorage is now used for ephemeral in-session edit drafts only.
- `secretInfoLabel` exists as config/default scaffolding only and is not yet a runtime-wide configurable behavior.

### Immediate Interpretation

`1.4.21` should be treated as a narrow baseline-hardening revision, not as completion of a roadmap package. The next active roadmap work begins from that hardened baseline.

---

## Package Status Board

| Package | Name | Status | Branch / Version | Notes |
|---|---|---|---|---|
| P1 | Canonical Data Contract | In Progress | `1.4.21` baseline established | Body-only editing and live-boundary sanitization landed, but routed-state ownership, editable-vs-routed receipts, and formal downstream contract work are still open. |
| P2 | Config Registry and UI Wiring | Not Started |  | Small config scaffolding exists, but there is no full registry, complete UI wiring, or diagnostics-backed resolved config layer yet. |
| P3 | Routing and Splicing Engine | Not Started |  | No production routing into Memory, Author's Note, or Lorebook yet. |
| P4 | Dense Editor UI Surface | Not Started |  | Sidebar remains the current editing surface; larger editor migration has not started. |
| P5 | Prompt and Multi-Character Quality | Not Started |  | Current prompt behavior remains stable but broader quality and multi-character work has not started as a package. |
| P6 | Diagnostics, Sync, and Validation | Not Started |  | Stable diagnostics exist for the current sidebar tool, but route-state/sync-state validation work has not started. |

Status values: Not Started, In Progress, Blocked, Complete, Deferred

---

## Locked Decisions

Record only decisions that should carry forward into later chats.

- Canonical editable output is the source for routing.
- Raw model output remains diagnostic only.
- Sidebar remains the control and monitoring surface.
- Dense editing moves to a bottom-bar or alternate editor surface.
- Script-owned routing must use ownership markers and safe replacement only.
- Build new revisions from the stable live `.naiscript` base, not by adopting broad fork rewrites.
- `1.4.21` remains a narrow hardening pass, not a roadmap-phase rewrite.
- Secret-info editing is body-only in the current UX.
- The visible `Secret information` label is conceptually separate from the editable body.
- `secretInfoLabel` exists now as scaffolding only; full label configurability is deferred.
- tempStorage is valid for uncommitted draft convenience only and should not be treated as durable recovery.
- Manual edits are authoritative user-authored continuity state until the user explicitly replaces them by refreshing again.
- If the user advances the turn without replacing the current edited output, that active edited output is the value that should influence continuity.
- If the user refreshes again before advancing the turn, the newly refreshed output becomes the active carried-forward value.

---

## Open Decisions

Use this section for unresolved design choices that affect later packages.

- Lorebook routing model: one master entry, per-character entries, or hybrid.
- Manual edit behavior once routing exists: auto-sync, dirty-state only, or apply-on-command.
- Default multi-character mode.
- Author's Note scope and length budget.
- Route apply behavior on refresh.
- Routing receipt shape: hash/signature only, version counter, or richer per-target receipt object.
- Whether branch-sensitive or turn-anchored routing receipts are needed beyond story-scoped storage.
- How much of the future config system should be schema-driven versus tightly hand-mapped UI.

---

## Package Detail Logs

## P1 — Canonical Data Contract

### Goal

Define raw output, editable output, derived display output, routed fragments, and routing receipts as separate layers with explicit ownership.

### Planned Deliverables

- Storage schema updates
- Routed-state container
- Editable-output canonicalization
- Dirty/sync state tracking

### Completion Check

- Manual edits change canonical editable output only
- Raw response remains preserved for diagnostics
- Every route can report what editable version it was derived from

### Progress Notes

- `1.4.21` established body-only editing for secret-info output while keeping the persisted current-output contract narrow.
- `1.4.21` added lightweight leading-label normalization/sanitization at active live boundaries.
- `1.4.21` deliberately did **not** broaden the persistent storage contract into a larger body/display schema redesign.
- The current baseline still lacks a routed-state container, route receipts, and explicit editable-to-routed ownership tracking.

### Handoff Summary

P1 has started, but only its baseline-hardening slice is done. The next real P1 work is to formalize routed-state ownership and receipts without collapsing the current four-layer architecture.

---

## P2 — Config Registry and UI Wiring

### Goal

Replace scattered flags with a validated config registry where every config has storage, UI, runtime behavior, and diagnostics visibility.

### Planned Deliverables

- Central config schema
- UI-generated controls or tightly mapped controls
- Runtime consumers for every config
- Diagnostics view for resolved config values

### Completion Check

- No dead config flags remain
- Changing a config in UI affects behavior or clearly marks pending apply state

### Progress Notes

- `1.4.21` added only limited config/default scaffolding via `secretInfoLabel`.
- Full config completion has not started.
- Current config behavior should still be treated as partial and uneven until a dedicated package pass addresses it.

### Handoff Summary

Do not over-read `1.4.21` as config completion. P2 remains future work after the next P1 contract pass or alongside it if a narrow shared schema becomes necessary.

---

## P3 — Routing and Splicing Engine

### Goal

Route the canonical editable output into Memory, Author's Note, and Lorebook safely using target adapters and ownership markers.

### Planned Deliverables

- Shared route interface
- Preview/apply/clear/resync actions
- Target-specific transformers
- Ownership marker strategy

### Completion Check

- Script updates only its own blocks
- Route status can show synced, dirty, missing, or failed per target

### Progress Notes

- No production routing behavior exists yet.
- Current project direction is to route from canonical editable output, not raw output.
- Future routing must preserve same-turn reroll protection and must not mutate unrelated user-authored context surfaces.

### Handoff Summary

P3 has not started. When it begins, it should build outward from the hardened editable-output contract rather than from raw generation output.

---

## P4 — Dense Editor UI Surface

### Goal

Move dense prompt and output editing out of the crowded sidebar into a bottom-bar or alternate editor surface.

### Planned Deliverables

- Editor launcher controls
- Dedicated editor surfaces for prompt fields and editable output
- Save/apply/cancel behaviors
- Route preview editor surface

### Completion Check

- Sidebar remains usable as quick-control surface
- Dense fields are comfortably editable without crowding

### Progress Notes

- `1.4.21` improved the current sidebar edit behavior through body-only editing and temp draft handling.
- This is not the same as beginning the dense-editor migration.
- Sidebar remains the active operational surface for now.

### Handoff Summary

P4 remains future work. Keep the sidebar stable and lightweight until a dedicated editor surface is intentionally designed.

---

## P5 — Prompt and Multi-Character Quality

### Goal

Improve generation constraints, formatting, and character handling so output stays evidence-driven and structured.

### Planned Deliverables

- Revised default prompts
- Evidence/speculation constraints
- Multi-character ranking and formatting rules
- Output mode options

### Completion Check

- Output remains soft but grounded
- Multiple characters are handled intentionally instead of blending together

### Progress Notes

- No package-level P5 work has started.
- Existing prompt behavior remains the stabilized baseline that later work must preserve unless intentionally revised.

### Handoff Summary

Treat current prompt quality as stable baseline, not as completed roadmap quality work.

---

## P6 — Diagnostics, Sync, and Validation

### Goal

Expand diagnostics to explain generation state, edit state, route state, sync state, and repair actions.

### Planned Deliverables

- Raw vs editable vs routed inspection
- Per-target sync reporting
- Repair actions and status views
- Regression checklist

### Completion Check

- User can tell whether a problem came from generation, edit handling, routing, or UI sync
- Recovery actions exist without manual storage surgery

### Progress Notes

- Current diagnostics are valid for the present sidebar-first generation tool.
- Route-state diagnostics, sync receipts, and repair actions do not exist yet.
- Any future diagnostics expansion must stay last in the sidebar unless the user explicitly changes that rule.

### Handoff Summary

P6 remains future work and should follow the first real routing-state implementation.

---

## Regression Checklist

Run this before marking a package complete if it touches shared behavior.

- Reroll behavior still ignores same-turn contamination correctly
- Prompt preview still rebuilds correctly
- Editable output persists correctly
- Manual edits do not mutate raw diagnostics
- Manual edits remain the active carried-forward value unless the user explicitly refreshes again before the next turn
- Generated output can be refreshed/replaced without collapsing the current storage separation
- No unrelated UI panels broke
- Stored config survives reload
- Route previews match applied content
- Script-owned blocks replace cleanly without touching unrelated text

---

## Next Chat Handoff Block

Copy this into a new chat when moving to the next package.

```md
Project: Secret Info Panel
Roadmap Reference: SecretInfoPanel_Roadmap_v1_1.md
Progress Tracker: SecretInfoPanel_Progress_Tracker_v1_1.md
Current Working Version: 1.4.21
Completed Packages: none yet; baseline hardening through 1.4.21 established
Current Package To Build: P1 — Canonical Data Contract
Locked Decisions: canonical editable output routes downstream; raw output remains diagnostic; body-only editing stays; secretInfoLabel is scaffolding only; tempStorage drafts are ephemeral only; manual edits remain authoritative until explicitly replaced by refresh
Open Decisions Still Pending: lorebook routing model; manual edit sync model once routing exists; routing receipt shape; route apply behavior on refresh; default multi-character mode; Author's Note scope and length budget
Known Risks / Bugs: avoid collapsing four-layer storage separation; avoid breaking same-turn reroll protection; avoid turning narrow data-contract work into a broad rewrite
What Must Be Preserved: sidebar-first control surface, canonical editable-output routing rule, reroll protection, diagnostics last, narrow structural changes only
Requested Task For This Chat:
```
