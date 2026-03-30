# Secret Info Panel Script Architecture

## Purpose

Secret Info Panel is a NovelAI `.naiscript` project that generates and manages a private secret-information continuity block during story sessions.

The current live baseline is `1.5.1.6`. This is no longer the older sidebar-first `1.5.0.5` state. The live script now includes the `1.5.1` UI/config-groundwork packet on top of the preserved canonical-output and route-scaffold contracts.

This document describes the current implemented baseline on `main`. It should be read as architecture and contract guidance for the live script, not as a promise that every later roadmap goal is already implemented.

## Current baseline

- promoted live revision: `1.5.1.6`
- live script: `live/secret_info_panel_live.naiscript`
- promoted source artifact: `archive/code/secret_info_panel_v1_5_1_6.naiscript`
- current phase position: post-`1.5.1`, pre-`1.5.2`

The implementation now combines:

- preserved `1.5.0.x` canonical-output and routed-state contracts
- `1.5.1` UI/config groundwork and surface split
- preview-only output planning state
- prepared but not yet wired input-source controls for `1.5.2`

## Important initialization behavior

The current live script does **not** normally perform a destructive clean-slate reset on script load.

Interpret the current behavior this way:

- `storyStorage` is used for durable per-story working state
- `tempStorage` is used for ephemeral session and UI convenience state
- `onScriptsLoaded` restores the panel and clears stale auto-refresh markers when needed
- destructive reset now lives behind the guarded `Reset Panel State` action
- `initializeFreshSession()` still exists, but it is an explicit reset path, not the normal initialization path

Therefore, the current architecture should be read as:

- state containers and ownership boundaries are real and important
- durable panel state is expected to survive normal reloads
- destructive reset is explicit and guarded
- future work should preserve that distinction unless project direction changes deliberately later

## High-level architecture

The current script is best understood as seven layers:

1. **Prompt fields** - persistent user-editable prompt text and config-like values
2. **Current output contract** - committed canonical body plus derived display text
3. **Generated runtime state** - timestamps, raw response capture, prompt dynamic block, status, history, auto-refresh markers, and turn-seed counters
4. **Prompt preview** - rebuilt prompt artifact used as the generation source of truth inside the script
5. **Routed-state planner scaffold** - route target metadata, ownership placeholders, desired signatures, and preview-planning state for future routing
6. **Ephemeral edit-session state** - tempStorage-backed active field and draft values for the current session only
7. **UI runtime state** - workspace tab, runtime log, busy flags, and reset confirmation state

## Primary functional cycle

The current live cycle is:

1. load current state and rebuild the panel surfaces
2. read current story context from NovelAI during refresh
3. rebuild the prompt preview from the current inputs
4. call `generateWithStory`
5. receive one compact `Secret information:` paragraph
6. normalize the result into canonical body and derived display text
7. store diagnostics and append bounded history
8. expose the result through the current Secret Info workspace tab and sidebar summaries

The script still does **not** yet perform finished production routing into Memory, Author's Note, or Lorebook. It carries planning scaffolding for that later work.

## UI surface model

The current UI should be read as two coordinated surfaces.

### Sidebar

The sidebar is now the compact monitoring, advanced-controls, and diagnostics plane.

Its intended sections are:

- **Status Info**
- **Advanced**
- **Diagnostics**

The sidebar is no longer the dense editing surface.

### Workspace / script panel

The script panel is now the dense working and editing plane.

The top-row workspace tabs are:

- `Secret Info`
- `Prompting`
- `Inputs`
- `Outputs`
- `History`

Interpretation rule:

- this tab split is current baseline, not a future concept
- `Inputs` and `Outputs` were split early so `1.5.2` can build on stable surfaces rather than reshuffling the UI again

## Storage layout

### `storyStorage` keys

- `secret-info-panel-prompt-fields-v1`
- `secret-info-panel-current-output-v1`
- `secret-info-panel-generated-state-v1`
- `secret-info-panel-prompt-preview-v1`
- `secret-info-panel-routed-state-v1`

### `tempStorage` keys

- `secret-info-panel-runtime-log-v1`
- `secret-info-panel-edit-active-field-v1`
- `secret-info-panel-edit-secret-info-label-draft-v1`
- `secret-info-panel-edit-secret-info-draft-v1`
- `secret-info-panel-edit-output-wrapper-draft-v1`
- `secret-info-panel-edit-system-prompt-draft-v1`
- `secret-info-panel-edit-script-prompt-draft-v1`
- `secret-info-panel-workspace-tab-v1`

### Storage interpretation rule

- `storyStorage` holds durable per-story state
- `tempStorage` holds ephemeral draft or UI-session state
- avoid `historyStorage` unless a future setting is explicitly meant to follow undo/redo semantics

## Persistent data containers

### Prompt fields

Stores prompt text and config-like values such as:

- `scriptSystemPrompt`
- `scriptPrompt`
- `generationTemperature`
- `generationMaxTokens`
- `maxHistoryEntries`
- `autoRefreshEachTurn`
- `autoRefreshEveryTurns`
- `autoRefreshResetOnManualRefresh`
- `inputAutoDiscoverCharacters`
- `inputUseSelectedLorebooks`
- `outputWrapper`
- `secretInfoLabel`

### Current output

Stores the live output contract:

- `secretInfo`
- `canonicalBody`
- `derivedDisplayText`

### Generated state

Stores runtime and audit data such as:

- `lastUpdatedAt`
- `lastModel`
- `lastStatus`
- `lastError`
- `lastStoryTitle`
- `lastPromptDynamic`
- `lastRawResponse`
- `lastRawResponseBody`
- `canonicalContractVersion`
- `routedFragmentStatus`
- `routingReceiptStatus`
- history state
- auto-refresh state
- turn/seed counters

### Prompt preview

Stores `promptPreview`.

### Routed state

Stores route-layer planning scaffolding such as:

- `contractVersion`
- `adapterContractVersion`
- `sourceCanonicalSignature`
- `sourceCanonicalUpdatedAt`
- `routedFragmentStatus`
- `routingReceiptStatus`
- `lastReceiptSweepAt`
- target-level state for `memory`, `authorsNote`, and `lorebook`

Each target already carries fields for:

- adapter identity
- ownership markers
- desired signatures
- apply timestamps
- applied signatures
- receipt status
- receipt errors
- preview-planned fragments and plan status

## Canonical output contract

The current baseline should be read as distinct layers:

1. raw model response text
2. raw response body
3. committed canonical body
4. derived display text
5. routed preview / planner fragments

Key downstream rules:

- `canonicalBody` is the authoritative downstream source
- `derivedDisplayText` is rebuilt from committed canonical body
- `secretInfo` remains the visible stored display representation
- raw model output remains diagnostic only
- future routing must derive from committed canonical body, not raw output

## Secret Info editing model

The current live baseline includes explicit edit-session handling and separate label/body flows.

### Current editable surfaces

- Secret Info label
- Secret Info body
- output wrapper
- script system prompt
- script prompt

### Current rules

- one editable field is active at a time
- drafts live in `tempStorage` during the session
- saving commits changes back into `storyStorage`
- cancelling clears the temp draft state
- label editing is separate from body editing
- secret-info editing remains body-only at the canonical edit layer
- label-only saves do not create history entries
- label saves scrub trailing colon-like suffixes
- unsaved drafts do not silently become authoritative state

### Secret Info action model

Idle state:

- `Edit Label`
- `Edit Body`
- `Reset Label`
- `Clear Block`

Label edit state:

- `Save Label`
- `Cancel`

Body edit state:

- `Save Body`
- `Cancel`

## Prompt model

The generation prompt is built from three inputs:

- `scriptSystemPrompt`
- a dynamic prompt block rebuilt from runtime state
- `scriptPrompt`

The stored preview is rebuilt in this shape:

```text
SYSTEM:
[scriptSystemPrompt]

USER:
[lastPromptDynamic]

[scriptPrompt]
```

That stored prompt preview is the generation source of truth inside the script.

### Same-turn reroll protection

Same-turn reroll protection remains a core preserved behavior.

The generated state tracks:

- `storyTurnCounter`
- `promptSeedTurnCounter`
- `promptSeedSecretInfo`

Effect:

- rerolls during the same story turn reuse the same stable seed
- a completed new story turn allows the next refresh to advance from the carried-forward state
- refresh behavior is protected from same-turn recursive contamination

## Inputs model

The `Inputs` tab now exists as the prepared home for `1.5.2` source assembly work.

Current visible homes are:

- `Auto-discover characters`
- `Selected lorebooks`

Interpret these carefully:

- they are not mutually exclusive
- they are not functionally wired into generation yet
- they are structural placeholders for the next packet
- `Selected lorebooks` is intentionally basic and does not yet provide a built-in selector UI
- later lorebook selection is expected to use a custom selector workflow

## Outputs model

The `Outputs` tab now exists as a planning surface.

Current output-facing controls include:

- `Send to Memory`
- `Send to Author's Note`
- `Send to Lorebook`
- a shared editable `Wrapper` field

Interpret these carefully:

- the target toggles are planner state, not live output writes
- the wrapper is editable now, but it is a prepared config field rather than an active routing engine
- Outputs remains planning-only in the current baseline

## History and refresh model

### History

The current history surface includes:

- `maxHistoryEntries` under the `History` tab
- a slider and typed entry pathway
- explicit `Clear Log`
- bounded stored history entries

Current trimming behavior:

- changing the max history value does not immediately trim stored history
- trim occurs on the next refresh/save cycle that persists history

### Refresh

The current header controls include:

- `Refresh`
- auto-refresh toggle
- auto-refresh cadence field

Current rules:

- manual refresh remains the safe baseline
- auto-refresh remains opt-in only
- cadence is constrained to `1` through `15`
- cadence input is disabled while auto-refresh is disabled
- a visible toast appears when background refresh starts so users know the script is already working

## Diagnostics and planner visibility

Diagnostics is now a compact health and inspection surface rather than a raw dump box.

Current diagnostics cover:

- run state, updated time, and model
- output, canonical body, display text, and raw response lengths
- prompt and dynamic prompt lengths
- history counts and latest-entry checks
- auto-refresh state
- canonical contract and adapter contract versions
- route adapter summary
- route receipt summary
- route planner summary
- warnings
- expandable detail areas for last error, raw response, prompt dynamic block, contracts, routing, checks, and validation notes

This means the route scaffolding is inspectable even though full routing is not yet implemented.

## Hook lifecycle

The live script uses these NovelAI hook surfaces:

- `onResponse`
- `onGenerationRequested`
- `onGenerationEnd`
- `onScriptsLoaded`

They are used to:

- track completed story generations
- coordinate auto-refresh behavior
- distinguish user story turns from script work
- prevent older auto-refresh work from colliding with newer story generations
- restore the panel on script load without performing an automatic destructive reset

## Current non-features

The current implementation still does not provide a finished production system for:

- applying routed output into Memory
- applying routed output into Author's Note
- applying routed output into Lorebook entries
- receipt validation against live target content
- ownership-marker splicing into live targets
- manual lorebook selector UX
- multi-character output expansion
- non-character generalized source handling
- finished live source assembly for `Inputs`
- broad routing/default-behavior architecture
- rigid JSON extraction
- broad compatibility scaffolding across older archived revisions

## Implemented baseline summary

At `1.5.1.6`, the script is best understood as:

- a private continuity generator with preserved canonical-output contracts
- a two-surface UI with compact sidebar monitoring and dense workspace editing
- a split workspace with stable `Secret Info`, `Prompting`, `Inputs`, `Outputs`, and `History` homes
- a system with separate label/body editing and explicit edit-session handling
- a planner-ready route scaffold for future targets
- a project that has prepared the next packet without implementing live routing yet

## Interpretation guardrails

When reading this architecture against the roadmap:

- do not treat routed-state scaffolding or target toggles as proof that live routing already exists
- do not treat the `Inputs` tab as proof that source assembly is already wired
- do treat the `1.5.1` surface split and config placement decisions as current baseline contracts
- do treat the guarded reset behavior as current intended behavior
- do treat canonical output ownership, reroll protection, and route-planner scaffolding as real baseline contracts
- do treat `1.5.2` input assembly and lorebook discovery work as the next major packet layered on top of this baseline
