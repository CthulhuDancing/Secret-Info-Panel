# Secret Info Panel Roadmap

## Purpose

This document describes the major work that remains after the current `1.5.0.5` live baseline.

It should be read as future direction only. If documents disagree, use them in this order:

1. `live/secret_info_panel_live.naiscript`
2. `live/current_version.md`
3. `docs/changelog.md`
4. this document and the rest of the current docs

## Current Confirmed Baseline

The live `1.5.0.5` baseline already includes:

- split prompt fields
- separate prompt preview storage
- separate current output, generated runtime state, and routed-state storage
- canonical body vs derived display text separation
- preserved raw diagnostic response layers
- same-turn reroll seed protection
- sidebar editing for secret-info body, system prompt, and script prompt
- tempStorage-backed edit-session handling
- diagnostics that expose route adapter and receipt summaries
- route-layer scaffolding for `memory`, `authorsNote`, and `lorebook`

The project is therefore past the older pre-contract baseline. The next major refactor should extend the current state contracts, not discard them.

## Current Interpretation

The current docs and live script already support a stronger baseline than the older roadmap language implied. The next major revision should not jump straight to finished live routing.

The next packet should first strengthen:

- authoritative-state ownership
- edit-session commit semantics
- UI surface roles
- routing-ready planning data
- injection readiness without early context contamination

That groundwork should make later prompt engineering, lorebook work, and live injection easier to validate and safer to ship.

## Confirmed Target Outcomes

The project should evolve into a lightweight NovelAI Storyteller userscript for **runtime present-state generation**.

Its primary use remains **character thoughts and reflections**, but the deeper project identity is broader: a practical tool for continuously morphing hidden or semi-hidden state information that the user can review, edit, and eventually route into story-context surfaces.

The intended end state includes:

- **two real modes**
  - silent read-along for inspiration
  - continuity mode with future routing back into context
- **manual refresh as the safe baseline**
- **auto-refresh as an opt-in feature only**
- **editable output with explicit ownership rules**
- **sidebar as the compact monitoring/control surface**
- **script panel as the dense working/editing surface**
- **future routing to Memory, always-on Lorebook, and Author's Note**
- **future hybrid character-source logic**
- **repo-manageable project structure with low break risk**

## Design Principles To Preserve

1. Keep the canonical-output contract intact.
2. Keep same-turn reroll protection intact.
3. Do not contaminate generated turns during validation.
4. Keep story-scoped persistence reliable across machines.
5. Treat raw model output as diagnostic, not authoritative truth.
6. Make user edits authoritative only after explicit save.
7. Keep UI roles clear and readable.
8. Prefer narrow structural changes over broad rewrites.
9. Keep the project practical, teachable, and repo-manageable.

## Authoritative State Model To Target

The next refactor should make this state ladder explicit:

1. **Raw generation**
   - diagnostic only
   - preserved for fallback and debugging
2. **Uncommitted working draft**
   - temporary edit-session state
   - never silently becomes canonical
3. **Committed canonical body**
   - authoritative downstream source for future routing
4. **Derived display text**
   - rebuilt from canonical body
   - not the editable truth
5. **Future route fragments / route plans**
   - derived from committed canonical body only

This should become the stable ownership model before live route apply is introduced.

## What To Implement Now

### A. Authoritative Output and Edit Ownership Cleanup

Required outcomes:

- explicit save/cancel semantics for user edits
- unsaved edits remain draft-only
- `rawLastOutput` remains diagnostic and fallback
- canonical body changes only on explicit commit
- derived display text rebuilds from committed canonical body

### B. Dense Editor UI Migration

Required outcomes:

- sidebar remains for refresh controls, summaries, route state, and diagnostics
- script panel becomes the main working surface for prompt fields, canonical body editing, and future previews
- temporary UI awkwardness during the split is acceptable only if resolved quickly in follow-up subrevisions

### C. Routing-Ready Planner Layer

Required outcomes:

- keep routed-state scaffolding
- compute target-ready preview fragments from committed canonical body
- support preview-only route planning before live apply
- represent dirty/current/planned target state clearly
- expose enough route-plan state for diagnostics and storage inspection

### D. Injection-Ready Config Tightening

Required outcomes:

- formalize the small set of config decisions needed for later routing
- keep them tightly mapped rather than over-schema-driven
- support future output mode, target enablement, route behavior, and editor-surface preferences without enabling live apply yet

## What To Defer

### Defer Until After the Next Groundwork Packet

- full target apply / clear / resync into Memory, Author's Note, and Lorebook
- ownership-marker splicing into live targets
- receipt validation against real target content
- full Lorebook character discovery system
- multi-character formatting expansion
- repair tooling beyond what is needed to validate the new contracts
- preset prompt packs and broader convenience features
- broad migration layers or compatibility scaffolding for older archived revisions

## Top 3 Next Code Changes In `live/secret_info_panel_live.naiscript`

### 1. Make edit ownership explicit

Implement a stricter distinction between:

- raw generation
- unsaved draft
- committed canonical body
- derived display text

This is the highest-value code change because it improves validation safety, future routing safety, and prompt-work clarity.

### 2. Introduce the script panel as the main working surface

Move dense editing out of the sidebar first, keeping the sidebar as the stable control and monitoring surface.

The first candidates to move are:

- canonical body editor
- system prompt editor
- script prompt editor
- prompt preview / future route preview
- save/cancel/commit actions

### 3. Upgrade routed-state from placeholder scaffold to preview planner

Do not implement live apply yet. Instead:

- compute per-target desired fragments from committed canonical body
- track dirty/planned/current target status
- expose Memory / always-on Lorebook / Author's Note previews
- support mode-based and per-target preferences in planning state only

## Anti-Sprawl Guardrails

Do **not** build these yet:

- finished production target apply engine
- generalized schema/config framework
- full character-source discovery engine
- multi-character expansion logic
- lorebook mutation system
- heavy modal/window-first redesign
- universal runtime-state framework abstractions
- broad compatibility code for older revisions
- rigid extractor / JSON-schema style output redesign

## Validation Plan By Phase

### Phase 1 - Authority and Edit-Session Refactor

Validation focus:

- generate -> edit draft -> cancel -> reload
- generate -> edit draft -> save -> reload
- failure state where generation succeeded but commit did not happen
- `storyStorage` inspection for ownership correctness

Success indicators:

- unsaved edits never become durable truth
- canonical body persists only on explicit save
- raw fallback remains intact
- derived display rebuilds correctly

### Phase 2 - UI Split

Validation focus:

- multi-machine reload cycles
- dense prompt/output edits in the script panel
- sidebar readability during and after the split
- persistence of the new working/editing flow

Success indicators:

- sidebar remains compact and readable
- script panel materially improves editing comfort
- no long-lived split-state confusion between two competing editors

### Phase 3 - Routing-Ready Planner

Validation focus:

- canonical body edits produce deterministic target previews
- target dirty/current/planned state transitions remain coherent
- route-plan state stays analyzable in `storyStorage`
- reroll protection still behaves correctly

Success indicators:

- each target can answer "what would be sent"
- desired signatures or equivalent target-plan identifiers update predictably
- no live story-context contamination yet

### Phase 4 - Lorebook and Source Groundwork

Validation focus:

- controlled tests for keyed, always-on, and inferred source strategies
- character-source decisions remain understandable
- route planning can represent always-on Lorebook as a future lane

Success indicators:

- source selection logic is understandable and testable
- always-on Lorebook can be planned without forcing live routing yet
- complexity stays proportional to the project size

### Phase 5 - Prompt and Injection Co-Development

Validation focus:

- target-by-target route application tests
- preservation of unrelated user-authored content
- receipt and ownership checks
- escalation/drift testing with manual vs auto modes

Success indicators:

- live apply is reversible or repairable
- owned blocks are the only blocks touched
- safe defaults remain conservative

## Explicit Risk Notes

### State Risk

The largest immediate risk is allowing temp draft state or unsaved UI state to become silent durable truth.

### Routing Risk

Jumping straight into production apply would bind unstable ownership rules to Memory, Lorebook, and Author's Note too early.

### Edit-Session Risk

The useful parts of temp draft handling should remain, but commit semantics need to become stricter. The main failure modes are:

- drafts becoming annoying and fragile
- drafts accidentally becoming canonical without explicit save

### Reroll / Contamination Risk

This remains non-negotiable. Same-turn rerolls must stay anchored to stable turn-backed seed state, and future routing must derive from committed canonical body rather than raw output.

### Performance Risk

The most noticeable performance bottlenecks are expected to remain:

- UI refresh churn
- GLM API generation latency

The next packet should therefore avoid unnecessary UI rebuild work and keep generation triggering conservative.

## Future Routing Direction

When live routing is eventually introduced, the intended context-strength direction should be:

1. **Memory**
2. **Always-on Lorebook**
3. **Author's Note**

Normal keyed Lorebook remains relevant, but more as part of the character/source model than as the first live routing implementation.

The longer-term automation direction should be:

- manual baseline
- mode-based automation
- eventually per-target configurability

Safe defaults matter more than maximum automation.

## Character / Source Direction

The longer-term character-source model should be hybrid.

Preferred order of trust:

1. explicit or keyed source declarations
2. discoverable lorebook-backed character sources
3. story-text inference as fallback

This work should happen shortly before the main prompt-engineering and live routing phase rather than at the very start of the next packet.

## Recommended Phased Roadmap

### Phase A - Commit Semantics and Authority Cleanup

Make save/cancel behavior explicit and preserve raw output as diagnostic fallback only.

### Phase B - Script Panel Migration

Move dense working/editing surfaces out of the sidebar while preserving the sidebar as the stable control and monitoring plane.

### Phase C - Routing-Ready Planner

Extend routed-state into preview-only target planning without applying to live story-context surfaces yet.

### Phase D - Lorebook / Source Groundwork

Add the hybrid source model and always-on Lorebook planning lane.

### Phase E - Prompt and Limited Routing Co-Development

Begin prompt engineering and carefully scoped live routing once the ownership model, UI split, and preview planner are stable enough that tests remain interpretable.

### Phase F - Broader Injection and Multi-Character Growth

Expand routing semantics, target automation, and multi-character handling only after the earlier phases prove stable.

## Completion Criteria For The Next Major Revision

The next major revision should be considered complete when:

- explicit commit semantics exist for current output editing
- raw output, draft state, canonical body, and derived display text are clearly separated
- the script panel is the main dense working surface
- route planning previews exist for future targets without requiring live apply
- manual refresh remains dependable
- auto-refresh remains opt-in
- same-turn reroll protection still behaves correctly
- `storyStorage` remains sufficient for GPT-aided state diagnosis
- the project remains lightweight and readable enough to hand off revision-by-revision

## Final Direction

The correct direction is not to replace the project with a rigid extractor or an over-abstracted routing framework.

The stronger version of Secret Info Panel should remain:

- a soft continuity / interiority tool
- grounded in explicit canonical-output ownership
- safe to validate and evolve revision-by-revision
- increasingly routing-capable without becoming fragile
- practical for real users and still understandable as a learning project
