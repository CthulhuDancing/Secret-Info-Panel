# Secret Info Panel Roadmap

## Purpose

This document describes the major work that remains after the current `1.5.0.5` live baseline.

It should be read as future direction only. Anything already present in the live script belongs in the architecture doc and progress tracker, not here.

## Current Confirmed Baseline

The live baseline already includes:

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

The project is therefore past the older pre-contract `1.4.21` baseline, but it still does not have finished production routing or the later UI/editor redesign.

## Roadmap Goal

The project should evolve from a sidebar-first continuity generator with route scaffolding into a stable continuity-routing tool that can:

- treat canonical editable output as the downstream source of truth
- route safely into Memory, Author's Note, and Lorebook targets
- support stronger multi-character handling
- expose a complete validated config model
- move dense editing into a better editor surface
- provide clear sync, validation, and repair tooling

## Design Principles To Preserve

1. Keep the canonical-output contract intact.
2. Keep same-turn reroll protection intact.
3. Route from canonical editable output.
4. Keep UI density under control.
5. Prefer narrow structural changes.

## Core Remaining Workstreams

### Workstream A: Route Engine Completion

Required outcomes:

- implement target adapters for Memory, Author's Note, and Lorebook
- support preview, apply, clear, and resync actions
- preserve unrelated user-authored content
- make ownership markers enforceable rather than placeholder-only
- upgrade receipt state from placeholder tracking into real sync receipts

### Workstream B: Config Registry Completion

Required outcomes:

- every real config has stored value, UI control, runtime consumer, and diagnostics visibility
- formalize route target enablement, route apply behavior, manual edit sync behavior, output mode, multi-character formatting, evidence strictness, diagnostics verbosity, and future editor-surface preferences

### Workstream C: Dense Editor UI Migration

Target result:

- sidebar remains for refresh, toggles, summaries, and diagnostics
- dense editors move to better editing surfaces for prompt fields, canonical output, and route previews

### Workstream D: Production Routing Semantics

Decisions still to lock:

- Lorebook routing model
- manual edit sync behavior
- route apply behavior on refresh
- target ownership marker strategy
- receipt shape

### Workstream E: Prompt and Multi-Character Quality

Required outcomes:

- improve evidence ranking and character selection
- reduce unsupported certainty and future-action bleed
- support intentional multi-character output modes
- make output easier to split into route fragments without heavy cleanup

### Workstream F: Validation and Repair Tooling

Diagnostics should eventually answer:

- what the raw response was
- what the canonical body is
- what the derived display layer is
- what each target would receive
- which targets are in sync, dirty, missing, or failed
- what repair or reapply actions are available

## Highest-Leverage Immediate Next Step

The highest-leverage next step is:

**complete the route engine on top of the already-landed canonical-output and routed-state contract**

That means:

- keeping canonical body as the route source
- converting route adapters from metadata placeholders into working target adapters
- implementing real ownership and receipt updates
- exposing route previews and resync flows before large UI migration work

## Completion Criteria For The Next Major Revision

The next major revision should be considered complete when:

- route targets can be previewed and applied safely
- canonical editable output drives all downstream routing
- ownership markers protect unrelated user-authored content
- receipts reflect actual sync state rather than placeholder-only status
- diagnostics can explain route state clearly
- same-turn reroll protection still behaves correctly after routing is added

## Final Direction

The correct direction is not to replace the project with a rigid extractor. The stronger version of Secret Info Panel should remain a soft continuity tool with explicit canonical-output ownership, safe target routing, better editor surfaces, better multi-character handling, and stronger validation visibility.
