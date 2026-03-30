# Secret Info Panel Progress Tracker

## Purpose

Use this document to track implementation progress across the current roadmap workstreams and to hand work forward without losing architectural decisions.

This tracker is aligned to the live `1.5.1.6` baseline, the current architecture document, the current roadmap, and the current `1.5.2` breakpoint handoff.

## Current project state

- Project: Secret Info Panel
- Roadmap Reference: `docs/secret_info_panel_roadmap.md`
- Architecture Reference: `docs/secret_info_panel_architecture.md`
- Current Working Version: `1.5.1.6`
- Current Focus: `1.5.2` input assembly and lorebook discovery groundwork
- Last Updated: `2026-03-29`

### Baseline state confirmed in `1.5.1.6`

- The workspace is now split into `Secret Info`, `Prompting`, `Inputs`, `Outputs`, and `History`.
- The sidebar is now the compact monitoring, advanced-controls, and diagnostics surface.
- Prompt fields, current output, generated state, prompt preview, and routed state are persisted separately.
- Current output includes canonical body and derived display text layers.
- Raw diagnostic response and raw response body are preserved separately.
- Same-turn reroll seed protection remains intact.
- Secret Info label editing and body editing are separate workflows.
- `Inputs` exists as the prepared home for source work but is not functionally wired yet.
- `Outputs` exists as a planning surface with target toggles and a shared wrapper field.
- Routed-state planner scaffolding exists for `memory`, `authorsNote`, and `lorebook`.
- Diagnostics expose contract, routing, and planner summaries.
- Normal script load does not perform a destructive reset; destructive reset is now explicit and guarded.

### Immediate interpretation

`1.5.1.6` is the current stable UI/config-groundwork endpoint.

The next work is:

- **not** more baseline UI reshuffling
- **not** direct production routing
- **not** broad source-management abstraction

The current packet is:

- `1.5.2` input assembly seam
- early lorebook discovery/source rules
- conservative source-state representation
- contamination-safe context integration and diagnostics support

Live output apply / clear / resync into Memory, Author's Note, and Lorebook remains deferred until that groundwork is stable.

## Workstream status board

| Workstream | Name | Status | Live Baseline | Notes |
|---|---|---|---|---|
| A | Docs and Source-of-Truth Alignment | Complete | `1.5.1.6` | Docs updated to reflect the promoted `1.5.1.6` baseline and `1.5.2` packet intent. |
| B | `1.5.1` UI/Config Groundwork | Complete | `1.5.1.6` | The workspace split, config placement, label/body workflow split, and guarded reset behavior are baseline. |
| C | `1.5.2` Input Assembly Seam | Not Started | prepared UI exists | `Inputs` now has a stable home, but no generation-path source assembly is wired yet. |
| D | Lorebook Discovery Rules Groundwork | Not Started | no functional source layer yet | Early source rules should remain conservative and character-biased. |
| E | Manual Lorebook Selection UX | Deferred | placeholder only | `Selected lorebooks` exists as planner state only; custom selector UX is later work. |
| F | Live Output Routing / Apply Engine | Deferred | planner scaffold only | Do not start live apply / clear / resync until C and D are stable. |
| G | Broader Source-Model Expansion | Deferred | no generalized source model yet | Do not broaden into non-character or large framework work during the first `1.5.2` packet. |
| H | Diagnostics and Validation Support for `1.5.2` | Not Started | diagnostics baseline exists | Extend diagnostics only as needed to inspect source assembly and contamination safety. |

Status values: Not Started, In Progress, Blocked, Complete, Deferred

## Locked decisions

- The live script is the implementation source of truth.
- The roadmap on `main` is the future-direction source of truth for current sequencing.
- `canonicalBody` is the downstream source for future routing.
- Raw model output remains diagnostic only.
- `derivedDisplayText` is rebuilt from committed canonical body.
- Unsaved draft state must remain draft-only.
- Sidebar remains the compact monitoring and diagnostics surface.
- Workspace / script panel remains the dense working and editing surface.
- `Inputs` is the next active architecture lane.
- `Outputs` remains planning-only for now.
- Manual refresh remains the safe baseline.
- Auto-refresh remains opt-in only.
- Same-turn rerolls must remain anchored to a stable seed for the current story turn.
- Narrow structural changes remain preferred over broad rewrites.
- Live routing must derive from committed canonical body, not raw diagnostic output.
- The current tab split and config homes should be treated as baseline unless a concrete UX problem appears.

## Open decisions

- Exact source-state shape for the first `1.5.2` packet
- Where the input assembly seam should live in the implementation, as long as it stays tied to context-building work
- How `Auto-discover characters` and `Selected lorebooks` should combine when both are enabled
- What the first conservative lorebook discovery rules should allow
- How the later custom lorebook selector should be introduced without reopening the baseline UI packet
- What extra diagnostics are actually needed for the first source-assembly pass
- How much explicit user-facing explanation the `Inputs` tab should provide once it becomes functional

## Workstream detail logs

### A - Docs and Source-of-Truth Alignment

**Status:** Complete

Completion target:

- docs reflect `1.5.1.6` rather than the older `1.5.0.5` baseline
- docs describe the current UI surface split accurately
- roadmap and tracker agree that `1.5.2` starts with input assembly and lorebook discovery groundwork

### B - `1.5.1` UI/Config Groundwork

**Status:** Complete

Completion target:

- stable workspace tabs
- stable config placement
- separate Secret Info label/body workflows
- prepared `Inputs` and `Outputs` homes
- guarded explicit reset behavior

This work is now baseline, not the next packet.

### C - `1.5.2` Input Assembly Seam

**Status:** Not Started

Completion target:

- define the code seam that assembles effective source input for generation
- make the current source assumptions inspectable
- keep the implementation narrow and low-risk
- preserve reroll and contamination protections

### D - Lorebook Discovery Rules Groundwork

**Status:** Not Started

Completion target:

- establish the first conservative source rules
- keep early behavior character-biased
- separate auto-discovery from explicitly selected lorebooks
- avoid pretending selector UX already exists

### E - Manual Lorebook Selection UX

**Status:** Deferred

Completion target when this workstream opens:

- custom selector flow exists for explicitly selected lorebooks
- selection UX layers onto the current `Inputs` surface without reopening the baseline packet

### F - Live Output Routing / Apply Engine

**Status:** Deferred

Completion target when this workstream opens:

- preview, apply, clear, and resync flows exist
- the script updates only its owned blocks
- per-target sync state reflects real applied content
- receipt validation works against live target state

This workstream should not begin until C and D have produced stable, testable contracts.

### G - Broader Source-Model Expansion

**Status:** Deferred

Completion target when this workstream opens:

- non-character or more generalized source handling can be introduced where justified
- the resulting model remains understandable and proportionate to the project size

This belongs after the first conservative `1.5.2` packet, not at the start of it.

### H - Diagnostics and Validation Support for `1.5.2`

**Status:** Not Started

Already present:

- current diagnostics cover run state, prompt checks, current output, raw response, canonical contract version, route adapter summary, route receipt summary, and route planner summary

Needed for `1.5.2` only if justified:

- source-assembly inspection that helps validate the new seam
- contamination-safety checks specific to source assembly
- minimal extra troubleshooting support for the new packet

## Regression checklist

Run this before marking later workstreams complete if they touch shared behavior.

- same-turn reroll protection still works
- prompt preview still rebuilds correctly
- canonical body persists correctly
- display text still rebuilds correctly from canonical body
- raw diagnostics remain preserved
- manual edits do not mutate raw diagnostics
- temp edit-session state does not become a durable source of truth
- route scaffolding fields remain coherent after save/load cycles
- `Inputs` state does not silently become live routing behavior
- `Outputs` remains planning-only until intentionally changed later
- diagnostics remain coherent after `1.5.2` seam changes

## Next handoff block

```md
Project: Secret Info Panel
Architecture Reference: docs/secret_info_panel_architecture.md
Roadmap Reference: docs/secret_info_panel_roadmap.md
Progress Tracker: docs/secret_info_panel_progress_tracker.md
Current Working Version: 1.5.1.6
Current Focus: 1.5.2 input assembly and lorebook discovery groundwork
Primary Active Workstreams: C input assembly seam; D lorebook discovery rules groundwork
Completed Baseline: 1.5.1 UI/config groundwork; workspace tab split; separate label/body edit flows; route-planner scaffolding; guarded destructive reset; compact sidebar plus dense workspace surface model
Deferred Until After First 1.5.2 Packet: E manual lorebook selector UX; F live output routing/apply engine; G broader source-model expansion
Locked Decisions: live script is source of truth; roadmap on main defines sequencing; canonical body routes downstream; raw output remains diagnostic; derived display text is rebuilt; unsaved drafts stay draft-only; sidebar stays compact monitoring surface; workspace remains dense editing surface; Inputs is the next active lane; Outputs remains planning-only; reroll seed protection must remain intact; manual refresh remains safe baseline; auto-refresh remains opt-in; narrow structural changes only
Open Decisions Still Pending: first source-state shape; exact input assembly seam; auto-discover vs selected-lorebooks interaction; first conservative lorebook rules; later custom selector path; minimal diagnostics additions needed for 1.5.2
Known Risks / Bugs: avoid source-framework sprawl; avoid weakening reroll protection; avoid collapsing Inputs and Outputs concepts; avoid jumping into live routing before source assembly stabilizes; avoid reopening already-settled UI packet decisions
What Must Be Preserved: 1.5.1.6 baseline contracts, canonical-body source-of-truth rule, reroll protection, Outputs planning-only status, current sidebar/workspace role split, narrow structural change preference
Requested Task For This Chat:
```

## Alignment note

This tracker should be read as a roadmap-aligned sequencing document for the current live baseline.

If documents disagree about what to build next:

1. `live/secret_info_panel_live.naiscript`
2. `live/current_version.md`
3. `docs/changelog.md`
4. `docs/secret_info_panel_roadmap.md`
5. `docs/secret_info_panel_architecture.md`
6. this tracker
7. `scratch/secret_info_panel_handoff_1_5_2_breakpoint.md`
