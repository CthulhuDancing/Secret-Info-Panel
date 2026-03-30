# Secret Info Panel 1.5 Mini-Map v1.1

This is a loose rail-guard for the 1.5 line. It is not a rigid sequence chart.

## Core 1.5 framing

- Keep the project **lightweight**
- Keep data structures **sturdy**
- Keep outputs to the story engine **simple**
- Keep processes **non-fragile**
- Prefer **simplicity over cleverness**
- Do **not** turn 1.5 into a prompt-rewrite line unless a structural requirement forces it
- Do **not** turn 1.5 into a UI-first line

## Main 1.5 direction

- Formalize the **canonical data contract**
- Build the **routing foundation** around that contract
- Keep the **1.4 architecture split** intact
- Preserve **reroll/turn stability**
- Treat **manual edits as first-class source truth**
- Add only **small durable storage additions**
- Keep diagnostics additions **minimal and practical**

## 1.5.0.x packet model

### 1.5.0.1 — Canonical layer formalization
- Lock the meanings of:
  - raw model response
  - canonical editable body
  - derived display text
  - routed fragments
  - routing receipts / sync state
- Preserve existing storage separation
- Add only the minimum new conceptual structure needed for later packets

### 1.5.0.2 — Routed-state container skeleton
- Add a new narrow routed-state storage object
- Keep it lightweight
- No real target application yet

### 1.5.0.3 — Canonical edit-to-route dirty tracking
- Canonical edits should mark route state dirty
- Manual edits remain first-class
- Still no heavy target application logic

### 1.5.0.4 — Shared route adapter contract
- Define the common shape for future Memory / AN / Lorebook adapters
- Keep it structural, not sprawling

### 1.5.0.5 — Sync receipt / ownership marker skeleton
- Define how the script knows what it owns
- Define how it knows whether a target is current, dirty, missing, or failed

### 1.5.0.6 — Minimal diagnostics exposure
- Show only enough to inspect the new contract cleanly
- Do not expand diagnostics just for the sake of completeness

## Meaning of 1.5.1

`1.5.1` should be the first **route-state-aware** version.

That means:
- the canonical contract is no longer just defined, it is actually being used consistently
- routed-state exists for real
- canonical edits can make routes dirty
- the system can tell whether a route is current, dirty, missing, or never applied
- ownership and sync markers exist in a real usable form

It does **not** mean:
- full Memory / Author's Note / Lorebook rollout
- UI redesign
- prompt overhaul
- broad config expansion

## Guardrails for the whole 1.5 line

- Preserve the 1.4 storage split
- Do not collapse canonical, runtime, and display concerns together
- Keep reroll/seed stability sacred
- Keep route-state additions small and explainable
- Favor understandable transformations over big bundled jumps
- Build the backend contract before redesigning the editing surface
