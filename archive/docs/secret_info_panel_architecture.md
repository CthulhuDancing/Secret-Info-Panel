# Secret Info Panel Script Architecture

## Purpose

Secret Info Panel is a NovelAI `.naiscript` project that generates and manages a private secret-information continuity block during story sessions.

The current live baseline is `1.5.0.5`. The project is still sidebar-first, but it is no longer only a pre-routing prototype. The live script now includes canonical output handling, preserved raw diagnostic layers, routed-state scaffolding, and tempStorage-backed edit-session handling.

This document describes the current implemented baseline on `main`. It should be read as architecture and contract guidance for the live script, not as a promise that every future roadmap goal is already implemented.

## Current Baseline

- promoted live revision: `1.5.0.5`
- live script: `live/secret_info_panel_live.naiscript`
- promoted source artifact: `archive/code/secret_info_panel_v1_5_0_5.naiscript`

The implementation now separates prompt fields, current output, generated/runtime state, prompt preview, and routed state.

## Important Initialization Behavior

The current live script intentionally performs a **fresh-session reset on script load**.

Interpret this carefully:

- the script does use `storyStorage` for its durable per-story working state during active use
- the script does use `tempStorage` for session-only convenience state
- but the current initialization flow intentionally clears the panel's own saved state and rebuilds a fresh baseline when the script loads

This is **intended behavior**, not an accidental persistence failure.

Therefore, the present architecture should be read as:

- state containers and ownership boundaries are real and important
- the current script still starts from a clean slate on initialization by design
- future work should preserve that deliberate reset behavior unless the project direction explicitly changes later

## High-Level Architecture

The current script is best understood as six layers:

1. **Prompt fields** - persistent user-editable prompt text and config-like values during active use
2. **Prompt preview** - rebuilt canonical prompt artifact used for generation
3. **Current output contract** - canonical editable body plus derived display text
4. **Generated runtime state** - timestamps, raw response capture, prompt dynamic block, status, history, and turn-seed counters
5. **Routed-state scaffolding** - route target metadata, ownership placeholders, signatures, and receipt placeholder state for future routing
6. **Ephemeral edit-session state** - tempStorage-backed active field and draft values for the current session only

## Primary Functional Cycle

The current live cycle is:

1. initialize a fresh session on script load
2. read current story context from NovelAI
3. rebuild the prompt preview from current inputs
4. call `generateWithStory`
5. receive one compact `Secret information:` paragraph
6. normalize the result into canonical body and derived display text
7. display it in the sidebar
8. save runtime diagnostics and append a bounded history entry

The script still does **not** yet perform finished production routing into Memory, Author's Note, or Lorebook. It now carries the route-layer contract needed for that later work.

## Prompt Model

The prompt is built from three inputs:

- `scriptSystemPrompt`
- dynamic prompt block rebuilt from runtime state
- `scriptPrompt`

The stored preview is rebuilt in this shape:

```text
SYSTEM:
[scriptSystemPrompt]

USER:
[lastPromptDynamic]

[scriptPrompt]

That stored prompt preview is the generation source of truth inside the script.

Same-Turn Reroll Protection

Same-turn reroll protection remains a core preserved behavior.

The script distinguishes between:

- current displayed output
- prompt seed output used for the current story turn

The generated state tracks:

- "storyTurnCounter"
- "promptSeedTurnCounter"
- "promptSeedSecretInfo"

Effect:

- rerolls during the same story turn reuse the same stable seed
- a completed new story turn allows the next refresh to advance from the carried-forward state
- refresh behavior is protected from same-turn recursive contamination

Storage Layout

storyStorage keys

- "secret-info-panel-prompt-fields-v1"
- "secret-info-panel-current-output-v1"
- "secret-info-panel-generated-state-v1"
- "secret-info-panel-prompt-preview-v1"
- "secret-info-panel-routed-state-v1"

tempStorage keys

- "secret-info-panel-runtime-log-v1"
- "secret-info-panel-edit-active-field-v1"
- "secret-info-panel-edit-secret-info-draft-v1"
- "secret-info-panel-edit-system-prompt-draft-v1"
- "secret-info-panel-edit-script-prompt-draft-v1"

Interpretation rule:

- storyStorage holds the panel's durable per-story working state during active use
- tempStorage holds ephemeral session convenience state only
- the current initialization path intentionally clears the panel's own saved state to start from a clean baseline on script load

Persistent Data Containers

Prompt fields

Stores prompt text and config-like values such as:

- "scriptSystemPrompt"
- "scriptPrompt"
- "maxHistoryEntries"
- "autoRefreshEachTurn"
- "useAuthorsNoteForSecretInfo"
- "lorebookOnlyCharacterDetection"
- "secretInfoLabel"

Current output

Stores the live output contract:

- "secretInfo"
- "canonicalBody"
- "derivedDisplayText"

Interpretation rule:

- "canonicalBody" is the editable continuity body
- "derivedDisplayText" is the rebuilt visible display layer
- "secretInfo" remains the currently stored visible representation

Generated state

Stores runtime and audit data such as:

- "lastUpdatedAt"
- "lastModel"
- "lastStatus"
- "lastError"
- "lastStoryTitle"
- "lastPromptDynamic"
- "lastRawResponse"
- "lastRawResponseBody"
- "canonicalContractVersion"
- "routedFragmentStatus"
- "routingReceiptStatus"
- history state
- turn/seed counters
- auto-refresh markers

Prompt preview

Stores "promptPreview".

Routed state

Stores route-layer scaffolding such as:

- "contractVersion"
- "adapterContractVersion"
- "sourceCanonicalSignature"
- "sourceCanonicalUpdatedAt"
- "routedFragmentStatus"
- "routingReceiptStatus"
- "lastReceiptSweepAt"
- target-level state for "memory", "authorsNote", and "lorebook"

Each target already carries fields for adapter identity, ownership markers, desired signatures, apply timestamps, applied signatures, receipt status, and receipt errors.

Canonical Output Contract

The current baseline should be read as distinct layers:

1. raw model response
2. raw response body
3. canonical body
4. derived display text
5. derived route fragments

Key downstream rule:

route from canonical body, not from raw diagnostic output

Edit-Session Model

The current live baseline includes explicit edit-session handling.

Current editable surfaces:

- secret-info output body
- script system prompt
- script prompt

Current rules:

- one dense field is edited at a time
- drafts live in tempStorage during the session
- saving commits changes back into storyStorage
- cancelling clears the temp draft state
- the visible "Secret information:" label is rebuilt automatically and is not part of the canonical body
- unsaved drafts should not silently become authoritative state

Sidebar UI Structure

The sidebar remains the active operating surface in "1.5.0.5".

Major sections currently include:

1. action controls
2. current secret-information output
3. recent history log
4. raw recent log viewer
5. script system prompt
6. script prompt
7. diagnostics

Diagnostics remains the final section in the sidebar.

Intended UI Direction

The current live implementation is still sidebar-first, but the broader project direction now treats the UI surfaces this way:

- sidebar = compact monitoring and control surface
- script panel = future dense working and editing surface

That script-panel migration is not yet implemented in the live file. The sidebar remains the actual working UI today.

Diagnostics Role

Diagnostics is a compact health and inspection surface rather than a raw dump box.

Current diagnostics cover:

- run state, updated time, and model
- output, canonical body, display text, and raw response lengths
- prompt and dynamic prompt lengths
- history counts and latest-entry checks
- auto-refresh state
- canonical contract and adapter contract versions
- route adapter summary
- route receipt summary
- warnings
- expandable detail areas for last error, raw response, dynamic prompt block, and validation notes

This means the route scaffolding is already inspectable even though full routing is not yet implemented.

Hook Lifecycle

The live script uses these NovelAI hook surfaces:

- "onResponse"
- "onGenerationRequested"
- "onGenerationEnd"
- "onScriptsLoaded"

They are used to:

- track completed story generations
- coordinate auto-refresh behavior
- distinguish user story turns from script work
- initialize a fresh session on script load
- prevent older auto-refresh work from colliding with newer story generations

Current Non-Features

The current implementation still does not provide a finished production system for:

- applying routed output into Memory
- applying routed output into Author's Note
- applying routed output into Lorebook entries
- dense script-panel editor migration
- preview-only target planning beyond scaffold/signature status
- rigid JSON extraction
- broad migration compatibility across old revisions

Implemented Baseline Summary

At "1.5.0.5", the script is best understood as:

- a sidebar-first private continuity generator
- using split prompt fields and stored prompt preview
- preserving raw diagnostic response layers
- maintaining canonical body vs derived display separation
- tracking runtime and history state separately
- carrying routed-state scaffolding for future target adapters
- protecting same-turn rerolls from recursive contamination
- using tempStorage-backed edit sessions for dense field edits
- intentionally starting from a clean session baseline on script load
- keeping diagnostics last in the sidebar

That is the current implemented architecture baseline that later docs should build from.

Interpretation Guardrails

When reading this architecture against the roadmap:

- do not treat routed-state scaffolding as proof that live routing is already implemented
- do not treat the storage split as proof that the script preserves its panel state unchanged through initialization
- do treat canonical output ownership, reroll protection, and route-layer scaffolding as real baseline contracts
- do treat clean-slate initialize behavior as intended current behavior
- do treat later UI migration and live routing as future work layered on top of the existing baseline
