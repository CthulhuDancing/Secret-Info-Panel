# Secret Info Panel Script Architecture

## Purpose

Secret Info Panel is a NovelAI `.naiscript` project that generates and manages a private secret-information continuity block during story sessions.

The current live baseline is `1.5.0.5`. The project is still sidebar-first, but it is no longer only a pre-routing prototype. The live script now includes canonical output handling, preserved raw diagnostic layers, routed-state scaffolding, and tempStorage-backed edit-session handling.

## Current Baseline

- promoted live revision: `1.5.0.5`
- live script: `live/secret_info_panel_live.naiscript`
- promoted source artifact: `archive/code/secret_info_panel_v1_5_0_5.naiscript`

The implementation now separates prompt fields, prompt preview, current output, generated runtime state, and routed state.

## High-Level Architecture

The current script is best understood as six layers:

1. **Prompt fields** - persistent user-editable prompt text and config-like values
2. **Prompt preview** - rebuilt canonical prompt artifact used for generation
3. **Current output contract** - canonical editable body plus derived display text
4. **Generated runtime state** - timestamps, raw response capture, prompt dynamic block, status, history, and turn-seed counters
5. **Routed-state scaffolding** - route target metadata, ownership placeholders, signatures, and receipt placeholder state for future routing
6. **Ephemeral edit-session state** - tempStorage-backed active field and draft values for the current session only

## Primary Functional Cycle

The current live cycle is:

1. read current story context from NovelAI
2. rebuild the prompt preview from current inputs
3. call `generateWithStory`
4. receive one compact `Secret information:` paragraph
5. normalize the result into canonical body and derived display text
6. display it in the sidebar
7. save runtime diagnostics and append a bounded history entry

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
```

That stored prompt preview is the generation source of truth inside the script.

## Same-Turn Reroll Protection

Same-turn reroll protection remains a core preserved behavior.

The script distinguishes between:

- current displayed output
- prompt seed output used for the current story turn

The generated state tracks:

- `storyTurnCounter`
- `promptSeedTurnCounter`
- `promptSeedSecretInfo`

Effect:

- rerolls during the same story turn reuse the same stable seed
- a completed new story turn allows the next refresh to advance from the carried-forward state

## Storage Layout

### storyStorage keys

- `secret-info-panel-prompt-fields-v1`
- `secret-info-panel-current-output-v1`
- `secret-info-panel-generated-state-v1`
- `secret-info-panel-prompt-preview-v1`
- `secret-info-panel-routed-state-v1`

### tempStorage keys

- `secret-info-panel-runtime-log-v1`
- `secret-info-panel-edit-active-field-v1`
- `secret-info-panel-edit-secret-info-draft-v1`
- `secret-info-panel-edit-system-prompt-draft-v1`
- `secret-info-panel-edit-script-prompt-draft-v1`

Interpretation rule:

- storyStorage holds durable per-story project state
- tempStorage holds ephemeral session convenience state only

## Persistent Data Containers

### Prompt fields

Stores prompt text and config-like values such as:

- `scriptSystemPrompt`
- `scriptPrompt`
- `maxHistoryEntries`
- `autoRefreshEachTurn`
- `useAuthorsNoteForSecretInfo`
- `lorebookOnlyCharacterDetection`
- `secretInfoLabel`

### Current output

Stores the live output contract:

- `secretInfo`
- `canonicalBody`
- `derivedDisplayText`

Interpretation rule:

- `canonicalBody` is the editable continuity body
- `derivedDisplayText` is the rebuilt visible display layer
- `secretInfo` remains the currently stored visible representation

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
- turn/seed counters
- auto-refresh markers

### Prompt preview

Stores `promptPreview`.

### Routed state

Stores route-layer scaffolding such as:

- `contractVersion`
- `adapterContractVersion`
- `sourceCanonicalSignature`
- `sourceCanonicalUpdatedAt`
- `routedFragmentStatus`
- `routingReceiptStatus`
- `lastReceiptSweepAt`
- target-level state for `memory`, `authorsNote`, and `lorebook`

Each target already carries fields for adapter identity, ownership markers, desired signatures, apply timestamps, applied signatures, receipt status, and receipt errors.

## Canonical Output Contract

The current baseline should be read as distinct layers:

1. raw model response
2. raw response body
3. canonical body
4. derived display text
5. derived route fragments

Key downstream rule:

**route from canonical body, not from raw diagnostic output**

## Edit-Session Model

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
- the visible `Secret information:` label is rebuilt automatically and is not part of the canonical body

## Sidebar UI Structure

The sidebar remains the active operating surface.

Major sections currently include:

1. action controls
2. current secret-information output
3. recent history log
4. script system prompt
5. script prompt
6. diagnostics

Diagnostics remains the final section in the sidebar.

## Diagnostics Role

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

## Hook Lifecycle

The live script uses these NovelAI hook surfaces:

- `onResponse`
- `onGenerationRequested`
- `onGenerationEnd`
- `onScriptsLoaded`

They are used to track completed story generations, coordinate auto-refresh behavior, distinguish user story turns from script work, and initialize a fresh session.

## Current Non-Features

The current implementation still does **not** provide a finished production system for:

- applying routed output into Memory
- applying routed output into Author's Note
- applying routed output into Lorebook entries
- dense bottom-bar editor migration
- rigid JSON extraction
- broad migration compatibility across old revisions

## Implemented Baseline Summary

At `1.5.0.5`, the script is best understood as:

- a sidebar-first private continuity generator
- using split prompt fields and stored prompt preview
- preserving raw diagnostic response layers
- maintaining canonical body vs derived display separation
- tracking runtime and history state separately
- carrying routed-state scaffolding for future target adapters
- protecting same-turn rerolls from recursive contamination
- using tempStorage-backed edit sessions for dense field edits
- keeping diagnostics last in the sidebar

That is the current implemented architecture baseline that later docs should build from.
