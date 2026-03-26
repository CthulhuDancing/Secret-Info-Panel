# Secret Info Panel Roadmap

## Purpose

This document defines the next major roadmap for the Secret Info Panel project based on the current architecture baseline, the latest workspace direction, and the current NovelAI scripting constraints.

It is written as a reference document for project sources, not as a one-off chat plan.

---

## Current Confirmed Baseline

The current script is still a sidebar-first secret-interiority generator. It already has split prompt fields, separate current output storage, separate generated/runtime state, separate prompt preview storage, same-turn reroll seed protection, limited sidebar history, and a diagnostics refactor. It is not yet treating Memory, Author's Note, or Lorebook injection as active production behavior.

The current architecture also already distinguishes between prompt inputs, a rebuilt prompt artifact, current displayed output, and generated runtime state. That separation should remain the foundation of future work.

---

## Roadmap Goal

The project should evolve from a sidebar-first testing tool into a controlled continuity system that can:

- fully expose and wire all intended config options
- generate output that remains soft, evidence-driven, and stable across rerolls
- support stronger multi-character handling
- let dense prompt and routing fields be edited in a bottom-bar style editor surface instead of forcing everything into the sidebar
- splice relevant derived output into one or more target context surfaces such as Memory, Author's Note, and an Always On Lorebook entry
- make manual edits safe and meaningful by clearly separating raw model output, editable output, display formatting, and routed output

This should be treated as a set of coordinated workstreams, not as a single straight-line sequence.

---

## Design Principles To Preserve

### 1. Keep the current separation of concerns

The current distinction between prompt fields, current output, generated state, and prompt preview is good and should be preserved. New features should extend that model rather than collapsing everything back into a mixed state object.

### 2. Keep same-turn reroll protection intact

The prompt seed model is one of the most valuable parts of the current script. Any routing, formatting, or multi-character work must not reintroduce recursive contamination from the newest same-turn reroll.

### 3. Route from editable output, not from raw output

The project should formally adopt this rule:

- raw model output is diagnostic
- current editable output is the user-facing canonical body
- display wrappers are derived
- routed fragments are derived from the editable output unless a future config explicitly says otherwise

This prevents the manual-edit surface from fighting the routing system.

### 4. Keep UI density under control

The sidebar should remain the place for status, summaries, recent history, quick controls, and diagnostics. Large dense editing surfaces should move into a better editing environment.

### 5. Prefer narrow structural changes over broad rewrites

This project already has a stable storage model and stable reroll behavior. The roadmap should build on that instead of replacing it.

---

## Core Workstreams

## Workstream A: Data Contract and Source-of-Truth Hardening

This is the foundation that makes the rest of the project stable.

### Objective

Define exactly which layer owns which data, and make all downstream features consume the correct layer.

### Required outcomes

- formalize the difference between:
  - raw model response
  - current editable secret-info body
  - derived display text
  - routed fragments
  - routing receipts / sync state
- keep the existing stable storage keys for prompt fields, current output, generated state, and prompt preview
- add narrowly scoped new storage only where needed for routing state and UI state
- remove any dependence on editable fields containing permanent display labels or prefixes
- ensure the editable field contains only the actual body content the user is meant to modify

### Recommended data model additions

Add a new routed-state container, for example:

- target enablement
- last routed fragments by target
- last apply status by target
- ownership markers / block ids
- hash or signature of the editable output that was last applied
- timestamp of last successful sync

Optional targeted use of history-aware storage can be considered for branch-sensitive receipts or turn-anchored state if it helps undo/redo behavior, but this should remain narrow and not replace the main story-scoped storage model.

### Acceptance standard

A manual edit to the output should update the canonical editable content without mutating the diagnostic raw response, and every downstream route should know which version of the text it is using.

---

## Workstream B: Config Completion and UI Wiring

Several config-like values already exist conceptually, but the next phase needs a real config system instead of scattered backend flags.

### Objective

Turn config from partial backend state into a complete validated registry with direct UI wiring and runtime effects.

### Required outcomes

Every config option should have all four parts:

1. stored default
2. UI control
3. runtime consumer
4. diagnostic visibility

### Confirmed config territory already implied by the current architecture

- maxHistoryEntries
- autoRefreshEachTurn
- useAuthorsNoteForSecretInfo
- lorebookOnlyCharacterDetection

### Additional config groups that should be formalized

- routing target selection
- routing apply mode
- routing ownership strategy
- manual edit auto-sync behavior
- output format mode
- multi-character formatting mode
- maximum character count per output
- evidence strictness / speculation strictness
- refresh behavior on story turn
- diagnostics verbosity
- editor surface preference for dense fields

### Implementation note

Use a central config schema and render the UI from that schema where possible. This reduces drift between storage, controls, and runtime behavior.

### Acceptance standard

No config should remain a dead flag. Changing a config in the UI should either affect behavior immediately or clearly require an apply action, and diagnostics should show the active resolved values.

---

## Workstream C: Routing and Splicing Engine

This is the major feature expansion that turns the script into a context-shaping tool instead of only a display tool.

### Objective

Build a routing engine that can transform the editable secret-info body into target-specific fragments and splice them safely into:

- Memory
- Author's Note
- an Always On Lorebook entry or entry set

### Required outcomes

- define route targets as adapters with a shared interface
- generate target-specific fragment shapes from the same canonical editable content
- support preview before apply
- support apply, refresh, clear, and resync
- avoid overwriting unrelated user text
- use explicit ownership markers so the script can update only what it owns

### Recommended routing contract

For each target, the system should answer:

- what content is being written
- where it will be written
- what existing script-owned block will be replaced
- whether non-script text exists around it
- whether the target is in sync with the current editable output

### Target-specific guidance

#### Memory

Use a compact stable summary format for durable continuity points only. Memory should not receive the full verbose body unless that becomes a deliberate config mode.

#### Author's Note

Use only short high-pressure guidance fragments that should sit near the generation frontier. Author's Note should be treated as a volatile tactical surface, not a long archive.

#### Always On Lorebook

Use structured character or continuity records that can persist independently of the immediate generation frontier. Decide early whether this should be:

- one master script-owned always-on entry
- one entry per tracked character
- one global continuity entry plus per-character entries

### Acceptance standard

Applying or refreshing routing should never destroy unrelated user-authored Memory, Author's Note, or Lorebook content. The script should always be able to identify and update only its own block.

---

## Workstream D: UI Redesign for Dense Editing

The current sidebar is doing too much for long prompt fields and dense continuity text.

### Objective

Keep the sidebar as a control and monitoring surface while moving dense editors into a more appropriate bottom-bar or windowed editing workflow.

### Target UI model

#### Sidebar should remain responsible for

- refresh and clear actions
- quick toggles
- current output summary
- recent history summary
- sync and route status
- diagnostics

#### Dense editing surfaces should move to a different editor surface

The preferred direction is a bottom-bar style editor workflow inspired by the attached example, with larger dedicated editing surfaces for:

- script system prompt
- script prompt
- current editable output
- routing preview
- multi-character breakdown review

### Recommended UI behavior

- compact launch buttons or tabs in the lower control area
- open-one-editor-at-a-time behavior for dense fields
- large multiline or code-editor style surfaces for prompt fields
- quick save / apply / cancel affordances
- a clear distinction between display view and edit view

### Important rule

Diagnostics should remain last in the sidebar even after the redesign.

### Acceptance standard

A user should be able to keep the sidebar focused on reading and control, while editing dense fields in a surface that does not compress or clutter the panel.

---

## Workstream E: Prompt and Generation Constraint Improvement

The current generation philosophy is good, but the next phase needs stronger constraints that still preserve the soft style.

### Objective

Improve prompt construction so output remains useful when routing and multi-character support become more capable.

### Required outcomes

- improve character evidence handling
- reduce unsupported interior claims
- reduce future invention and action planning bleed
- reduce duplication across rerolls and across multiple characters
- improve consistency when the story includes several active named entities
- make output easier to transform into routed fragments without losing tone

### Recommended prompt areas to refine

- evidence hierarchy from recent story text, lorebook-backed identity, and prior stable seed
- clear rules for uncertainty language
- explicit handling for absent or weakly evidenced characters
- multi-character output ordering rules
- explicit ban on turning the block into omniscient summary or hard verdict
- configurable routing-oriented formats that still read naturally

### Suggested output modes

Support at least these modes:

- single compact unified paragraph
- per-character mini blocks
- primary character plus secondary notes

### Acceptance standard

The prompt should produce output that is still soft and private in tone, but easier to split, route, and review without ad hoc cleanup.

---

## Workstream F: Multi-Character Handling

This is a major quality issue and should not be treated as only a formatting pass.

### Objective

Make the script capable of identifying, prioritizing, formatting, and routing multiple characters without flattening everything into one muddy paragraph.

### Required outcomes

- define a character candidate pipeline
- define ranking and cap rules
- define fallback behavior when character detection is ambiguous
- define output formats suitable for one, two, or several active characters
- define routing rules for character-specific vs global continuity

### Recommended character-selection sources

- manually pinned characters
- lorebook-backed candidates
- recent story evidence
- prior turn stable seed

### Recommended character-selection modes

- manual only
- lorebook only
- story evidence only
- hybrid weighted mode

### Formatting recommendations

Default to one of these two patterns:

#### Pattern 1: Per-character blocks

- character name
- short interiority / hidden state note
- optional one-line continuity flag

#### Pattern 2: Combined block with segments

- one unified block
- clear inline segment boundaries by character

Pattern 1 will usually be easier for routing and auditing.

### Acceptance standard

A scene with several active characters should no longer force the script into either a single vague blob or a brittle over-structured schema.

---

## Workstream G: Validation, Diagnostics, and Repair Tools

As routing and multi-surface ownership increase, validation becomes mandatory.

### Objective

Expand diagnostics from simple generation health into continuity and sync health.

### Required outcomes

Diagnostics should answer:

- what the current editable output is
- what the last raw model response was
- which targets are enabled
- which targets are in sync
- which targets failed last apply
- what content hash or version was last routed
- whether the prompt seed is stable and valid
- whether multi-character detection is using the expected mode

### Recommended repair actions

- re-derive routed fragments from current editable output
- re-apply all enabled targets
- clear only script-owned routed blocks
- rebuild prompt preview
- rebuild multi-character candidate set
- compare raw output vs editable output vs routed fragments

### Acceptance standard

When something looks wrong, the user should be able to identify whether the problem is generation, editing, formatting, routing, or target sync without opening storage exports.

---

## Recommended Build Bundles

These are not mandatory sequential phases. They are dependency-aware bundles that can overlap.

## Bundle 1: Foundation Bundle

Focus:

- Workstream A
- the schema portion of Workstream B
- the sync/repair skeleton of Workstream G

Why it matters:

This bundle defines the canonical data contract the rest of the project depends on.

## Bundle 2: Routing Bundle

Focus:

- Workstream C
- target ownership markers
- target previews
- apply / clear / resync flows

Why it matters:

This bundle unlocks the user-visible value of Memory, Author's Note, and Lorebook routing.

## Bundle 3: Multi-Character Quality Bundle

Focus:

- Workstream E
- Workstream F
- routing-aware output splitting

Why it matters:

This bundle prevents the new routing system from amplifying low-quality ambiguous output.

## Bundle 4: Dense Editor UI Bundle

Focus:

- Workstream D
- editor surface migration for long prompt and routing fields
- sidebar simplification

Why it matters:

This bundle makes the matured system comfortable to use rather than technically capable but awkward.

## Bundle 5: Final Validation Bundle

Focus:

- Workstream G completion
- edge-case testing
- clear ownership of apply sources
- cleanup of stale or legacy assumptions

Why it matters:

This bundle turns the project from an evolving prototype into a stable tool.

---

## Highest-Leverage Immediate Build Target

The single highest-leverage immediate target is:

**formalize the canonical editable-output contract and build the routing engine interface around that contract**

That means doing these things early:

- make the editable field body-only, not label-plus-body
- define routed fragment generation from the editable output
- define target adapters before full UI redesign
- define sync receipts and target ownership markers

This start point supports every one of the requested feature directions without forcing a rewrite.

---

## Key Decisions That Should Be Locked Before Heavy Implementation

### 1. Lorebook routing model

Choose one:

- one script-owned always-on entry
- one entry per character
- hybrid global plus per-character entries

### 2. Manual edit propagation rule

Choose one:

- manual edits immediately mark routes dirty but do not auto-apply
- manual edits auto-apply to enabled targets
- manual edits require explicit apply per target

### 3. Default multi-character output mode

Choose one:

- single unified paragraph
- per-character blocks
- hybrid primary-plus-secondary model

### 4. Author's Note role

Choose one:

- purely tactical short fragment
- compressed continuity reminder
- disabled by default except for explicit configurations

### 5. Refresh and routing coupling

Choose one:

- refresh only generates
- refresh generates and updates preview routes
- refresh generates and applies enabled routes automatically

---

## Completion Criteria For The Next Major Revision

The next major revision should be considered complete when all of the following are true:

- every config exposed by the project has a real UI control and real runtime effect
- Memory, Author's Note, and Lorebook routing all work through a shared routing contract
- manual edits are treated as first-class canonical content, not as temporary display tweaks
- dense fields are edited in a better surface than the crowded sidebar
- multi-character output is intentionally formatted and intentionally routed
- diagnostics can explain raw output, editable output, routed output, and sync state
- same-turn reroll stability still behaves correctly after all of the above

---

## Final Direction

The correct direction is not to turn the project into a rigid extractor. The stronger version of this script should remain a soft continuity tool with stable prompt shaping, explicit ownership of routed context, better editing surfaces, and better multi-character reasoning.

That keeps it aligned with the current architecture while still delivering the features the project now needs.
