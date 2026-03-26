# Secret Info Panel Progress Tracker

## Purpose

Use this document to track implementation progress across roadmap packages and to hand work forward into a new chat without losing architectural decisions.

---

## Current Project State

- Project: Secret Info Panel
- Roadmap Reference: `SecretInfoPanel_Roadmap_v1.md`
- Current Working Version:
- Current Focus Package:
- Last Completed Package:
- Last Updated:

---

## Package Status Board

| Package | Name | Status | Branch / Version | Notes |
|---|---|---|---|---|
| P1 | Canonical Data Contract | Not Started |  |  |
| P2 | Config Registry and UI Wiring | Not Started |  |  |
| P3 | Routing and Splicing Engine | Not Started |  |  |
| P4 | Dense Editor UI Surface | Not Started |  |  |
| P5 | Prompt and Multi-Character Quality | Not Started |  |  |
| P6 | Diagnostics, Sync, and Validation | Not Started |  |  |

Status values: Not Started, In Progress, Blocked, Complete, Deferred

---

## Locked Decisions

Record only decisions that should carry forward into later chats.

- Canonical editable output is the source for routing.
- Raw model output remains diagnostic only.
- Sidebar remains the control and monitoring surface.
- Dense editing moves to a bottom-bar or alternate editor surface.
- Script-owned routing must use ownership markers and safe replacement only.

Add project-specific decisions below:

- 
- 
- 

---

## Open Decisions

Use this section for unresolved design choices that affect later packages.

- Lorebook routing model: one master entry, per-character entries, or hybrid.
- Manual edit behavior: auto-sync, dirty-state only, or apply-on-command.
- Default multi-character mode.
- Author's Note scope and length budget.
- Route apply behavior on refresh.

Add new open decisions below:

- 
- 
- 

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

- 

### Handoff Summary

- 

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

- 

### Handoff Summary

- 

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

- 

### Handoff Summary

- 

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

- 

### Handoff Summary

- 

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

- 

### Handoff Summary

- 

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

- 

### Handoff Summary

- 

---

## Regression Checklist

Run this before marking a package complete if it touches shared behavior.

- Reroll behavior still ignores same-turn contamination correctly
- Prompt preview still rebuilds correctly
- Editable output persists correctly
- Manual edits do not mutate raw diagnostics
- No unrelated UI panels broke
- Stored config survives reload
- Route previews match applied content
- Script-owned blocks replace cleanly without touching unrelated text

---

## Next Chat Handoff Block

Copy this into a new chat when moving to the next package.

```md
Project: Secret Info Panel
Roadmap Reference: SecretInfoPanel_Roadmap_v1.md
Progress Tracker: SecretInfoPanel_Progress_Tracker_v1.md
Current Working Version: 
Completed Packages: 
Current Package To Build: 
Locked Decisions: 
Open Decisions Still Pending: 
Known Risks / Bugs: 
What Must Be Preserved: sidebar-first control surface, canonical editable-output routing rule, reroll protection, narrow structural changes only
Requested Task For This Chat: 
```
