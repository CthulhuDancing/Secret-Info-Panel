# Secret Info Panel Roadmap

## Purpose

This document describes the major work that remains after the current `1.5.1.6` live baseline.

It should be read as future direction only. If documents disagree, use them in this order:

1. `live/secret_info_panel_live.naiscript`
2. `live/current_version.md`
3. `docs/changelog.md`
4. this document and the rest of the current docs
5. relevant current handoff material in `scratch/`

## Current confirmed baseline

The live `1.5.1.6` baseline already includes:

- split workspace tabs for `Secret Info`, `Prompting`, `Inputs`, `Outputs`, and `History`
- compact sidebar roles for status, advanced controls, and diagnostics
- separate prompt fields, current output, generated runtime state, prompt preview, and routed state storage
- canonical body vs derived display text separation
- preserved raw diagnostic response layers
- same-turn reroll seed protection
- tempStorage-backed edit-session handling
- separate Secret Info label/body edit flows
- route-layer scaffolding and preview-planner state for `memory`, `authorsNote`, and `lorebook`
- a shared editable output wrapper field
- guarded destructive reset behavior rather than normal reset-on-load behavior
- placeholder input controls for `Auto-discover characters` and `Selected lorebooks`

The project is therefore past the older `1.5.0.5` groundwork framing. The next packet should extend the current `1.5.1.6` contracts rather than reopen them.

## Current interpretation

The correct next packet is **not** live routing.

The correct next packet is **`1.5.2` input assembly and lorebook discovery groundwork**.

Interpret that as:

- use the new `Inputs` tab as the visible home for the work
- preserve the current `1.5.1` UI/config decisions unless a concrete UX problem appears
- keep `Outputs` planning-only while source assembly matures
- stay conservative and character-biased at first
- prepare future routing by improving source assembly, not by turning on live apply early

## Design principles to preserve

1. Keep the canonical-output contract intact.
2. Keep same-turn reroll protection intact.
3. Do not contaminate generated turns during validation.
4. Keep authoritative working-state boundaries reliable.
5. Keep the current sidebar/workspace role split intact.
6. Treat raw model output as diagnostic, not authoritative truth.
7. Make user edits authoritative only after explicit save.
8. Keep `Inputs` and `Outputs` semantically distinct.
9. Prefer narrow structural changes over broad rewrites.
10. Keep the project practical, teachable, and repo-manageable.

## `1.5.2` packet intent

`1.5.2` should begin the **lorebook discovery rules and input-assembly rework**.

Strong interpretation of what `1.5.2` should focus on first:

### 1. Define the input assembly seam

Required outcomes:

- rework how the script assembles and feeds input to the secret-info generation call
- give the `Inputs` tab a real architectural purpose rather than leaving it as placeholders only
- keep the seam easy to inspect and debug
- avoid broad config frameworks or speculative source abstractions

### 2. Build the first source-model layer

Required outcomes:

- ordinary use remains `Auto-discover characters`
- the first explicit lorebook-input lane remains `Selected lorebooks`
- the early model stays conservative and character-biased
- avoid broadening too quickly into non-character source handling

### 3. Use the right technical seam

Required outcomes:

- prefer context-building work such as `onBeforeContextBuild` or equivalent source-assembly helpers
- keep source assembly separate from output routing
- keep contamination safety and turn-boundary reasoning explicit

### 4. Keep Outputs planning-only

Required outcomes:

- `Outputs` remains a structural preview/planning surface
- wrapper remains editable as prepared config, not as a live routing feature
- target toggles remain planner state rather than live writes

### 5. Preserve the `1.5.1` UI/config model

Required outcomes:

- no need to reopen the major tab split
- no need to move config homes again unless a concrete usability issue appears
- no need to collapse `Inputs` back into another tab

## What to implement now

### A. Input assembly seam

Required outcomes:

- define the code path that builds the effective generation inputs
- make the current source assumptions inspectable
- keep the seam narrow enough that future changes do not sprawl through unrelated systems

### B. Lorebook discovery rules groundwork

Required outcomes:

- define how lorebook-backed input discovery should be interpreted in the first conservative pass
- preserve a clear distinction between auto-discovery and explicitly selected lorebooks
- avoid pretending the selector UX already exists

### C. Source-state representation

Required outcomes:

- store only the small amount of durable source-selection state needed for the early packet
- keep the shape tight rather than schema-heavy
- make the state readable in repo handoffs and storage inspection

### D. Context and contamination safety validation

Required outcomes:

- verify that the new input assembly does not weaken same-turn reroll protection
- verify that the new source assembly does not leak unstable draft or planner state into generation
- extend diagnostics only as needed to inspect the new seam

## What to defer

### Defer until after the first `1.5.2` groundwork packet

- live apply / clear / resync into Memory, Author's Note, and Lorebook
- receipt validation against real target content
- ownership-marker splicing into live targets
- per-target wrapper settings
- advanced output write modes
- finished lorebook selector UX
- multi-character expansion logic
- non-character generalized source handling
- lorebook mutation workflows
- broad routing/default-behavior architecture
- rigid extractor / schema-style redesign
- broad compatibility layers for older archived revisions

## Anti-sprawl guardrails

Do **not** build these yet:

- finished production routing engine
- large generalized source framework
- universal config schema layer
- heavy modal or window-first redesign
- full multi-character or world-state expansion
- broad compatibility code for older revisions
- speculative lorebook-management tooling

## Recommended phased sequence

### Phase A - Input assembly seam

Create the narrow code seam that defines what source material is assembled and how it reaches generation.

### Phase B - Conservative source model

Wire the first limited source rules around auto-discovered characters and selected lorebooks without over-generalizing.

### Phase C - Diagnostics and validation support

Add only the inspection needed to confirm that source assembly is doing what the packet intends.

### Phase D - Selector and UX follow-up

Only after the early source model is stable should the project begin a custom lorebook selector workflow.

### Phase E - Routing follow-up

Only after the source model is understandable and stable should output-routing work resume.

## Validation focus for `1.5.2`

### Input assembly validation

- confirm the effective source assembly is understandable
- confirm manual refresh still behaves safely
- confirm auto-refresh still remains opt-in and bounded

### Contamination validation

- same-turn reroll protection still behaves correctly
- prompt seed behavior remains stable
- draft state does not become generation truth
- planner state does not silently become applied output state

### Source-model validation

- `Auto-discover characters` and `Selected lorebooks` remain distinct concepts
- the early model stays conservative instead of pulling in too much material
- later selector UX can be layered in without rewriting the packet

## Explicit risk notes

### Source-sprawl risk

The largest immediate risk is turning `1.5.2` into a broad source-management framework instead of a narrow input-assembly packet.

### Contamination risk

Jumping into context-building work too loosely could weaken the current reroll and seed protections.

### UI churn risk

Reopening the tab structure or config placement too early would re-spend effort that `1.5.1` already stabilized.

### Routing risk

Moving into live output routing before the input model is stable would skip the intended packet order and make failures harder to interpret.

## Future direction after `1.5.2`

Once the first `1.5.2` packet is stable, the likely later sequence is:

1. custom lorebook selector UX
2. broader source-model refinement where justified
3. prompt and source co-development on top of the stabilized seam
4. carefully scoped live routing work

That later work should still preserve:

- canonical-body ownership
- planner-before-apply sequencing
- conservative safe defaults
- low break risk

## Final direction

The stronger version of Secret Info Panel should remain:

- a soft continuity / interiority tool
- grounded in explicit canonical-output ownership
- safe to validate packet-by-packet
- increasingly capable without becoming over-abstracted
- practical for real users and still understandable as a learning project

The correct next move is therefore to make `1.5.2` about **input assembly and lorebook discovery rules**, not about live output routing.
