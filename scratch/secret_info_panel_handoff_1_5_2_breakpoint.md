# Full Breakpoint Handoff

## Scope
This handoff closes out the `1.5.1` UI/config-groundwork family and bridges into `1.5.2`, which should begin the lorebook discovery rules and input-assembly rework.

The checkpoint covers:
- the completed `1.5.1.1` through `1.5.1.6` UI/config work done in this chat
- the current intended UI surface model
- current config placement decisions
- the preserved contracts that still must not be disturbed
- the intended starting direction for `1.5.2`

This handoff is written so the next agent can begin `1.5.2` without having to reconstruct why the UI looks the way it does or which future lanes were deliberately prepared.

## Source Basis and Confidence
- Authoritative basis: the latest working chat-built artifact `secret_info_panel_v1_5_1_6.naiscript`; the prior repo breakpoint handoff `scratch/secret_info_panel_handoff_1_5_1_breakpoint.md`
- Supporting basis: prior `1.5.1` chat decisions and patch sequence in this chat; `live/current_version.md`; `docs/secret_info_panel_roadmap.md`; `docs/secret_info_panel_architecture.md`; `docs/secret_info_panel_progress_tracker.md`
- Inferred context: exact `1.5.2` sequencing and how the new Inputs/Outputs tabs should evolve beyond placeholder state
- Confidence: high

## Current Baseline
Current chat-built working endpoint: **`1.5.1.6`**

Important status note:
- this is the latest working artifact produced in chat, not a claimed repo-promoted baseline unless/until the user promotes it
- the prior repo-family endpoint before this work was `1.5.0.7c`
- `1.5.1` in this chat should be treated as a completed UI/config-groundwork packet layered on top of the preserved `1.5.0.x` state-contract work

Completed `1.5.1` subrevisions:
- `1.5.1.1` — action ownership and relocation
- `1.5.1.2` — copy compression
- `1.5.1.3` — workspace fit/space control
- `1.5.1.4` — current config-home placement and initialization cleanup
- `1.5.1.5` — Inputs/Outputs tab split, wrapper home, reset confirmation, and early `1.5.2` UI bridge
- `1.5.1.5a` — Secret Info label-row layout polish
- `1.5.1.5b` — removed the visible printed colon from the Secret Info label row
- `1.5.1.6` — unified Secret Info action area below the label/body fields with cleaner parallel label/body workflows

## Stable Contracts and Rules
These remain preserved and should stay intact in `1.5.2` unless changed deliberately and explicitly:

- `canonicalBody` remains the authoritative downstream source.
- `derivedDisplayText` is rebuilt from committed canonical body.
- `secretInfo` remains the visible stored display representation.
- raw model output remains diagnostic only.
- unsaved drafts remain draft-only until explicit save.
- secret-info editing remains body-only at the canonical edit layer.
- label editing is separate from body editing.
- label-only saves do not generate history entries.
- same-turn reroll protection remains intact.
- future routing must derive from committed canonical body, not raw output.
- routing/planning remains preview-only; no live apply exists yet.
- clean-slate initialization on script load is no longer the normal behavior; explicit destructive reset now lives behind a guarded UI button.
- sidebar remains the compact monitoring + advanced + diagnostics plane.
- over-input workspace remains the dense working/editing plane.
- narrow structural changes remain preferred over broad rewrites.

Storage and state-use rules to carry into `1.5.2`:
- durable per-story settings should go in `storyStorage`
- ephemeral UI/edit-session state should stay in `tempStorage`
- avoid using `historyStorage` unless a setting is meant to follow undo/redo semantics

## Authoritative References
Treat these as the primary references for continuing into `1.5.2`:

1. `secret_info_panel_v1_5_1_6.naiscript` — latest chat-built working artifact
2. `scratch/secret_info_panel_handoff_1_5_1_breakpoint.md` — prior durable phase handoff
3. `live/current_version.md` — prior promoted repo baseline note
4. `docs/secret_info_panel_roadmap.md` — future-direction guardrails
5. `docs/secret_info_panel_architecture.md` — still useful for preserved contracts, but stale relative to the latest chat-built UI
6. `docs/secret_info_panel_progress_tracker.md` — useful for earlier workstream framing, but stale relative to current chat-built UI
7. this handoff

Interpretation note:
- use the latest artifact plus the prior `1.5.1` breakpoint handoff first
- use roadmap/architecture as guardrails, not as a perfect description of the now-latest UI

## Current State
The UI/config-groundwork family is complete enough to begin `1.5.2`.

### Current workspace tab model
Top-row tabs now exist as:
- Secret Info
- Prompting
- Inputs
- Outputs
- History

This split was done deliberately now so `1.5.2` does not have to keep reshuffling the operating surface.

### Secret Info
Current state:
- always-visible label field
- label and body have separate edit workflows
- only one of label/body can be editable at a time
- `Reset Label` exists and resets to `Secret Information`
- visible printed colon was removed from the label row
- the Secret Info action layout is now unified below both fields

Secret Info action model now reads as:
- idle state: `Edit Label` / `Edit Body` / `Reset Label` / `Clear Block`
- label edit state: `Save Label` / `Cancel`
- body edit state: `Save Body` / `Cancel`

Important behavior:
- label save is separate from body save
- label save scrubs trailing colon-like suffixes
- label-only changes do not create history entries

### Prompting
Prompt text remains content, not config.

Current Prompting-related rule:
- raw prompt editors stay as editable content surfaces
- prompt-like defaults/configs should be treated separately from raw prompt text

### Inputs
Current state:
- dedicated Inputs tab exists
- first two visible homes are:
  - `Auto-discover characters`
  - `Selected lorebooks`
- these are **not mutually exclusive**
- they are **not functionally wired yet**
- `Selected lorebooks` remains intentionally basic; no baked-in lorebook picker exists, and a custom selector is expected later

Important interpretation:
- this tab is the prepared home for `1.5.2` input discovery/source work
- the current controls are structural placeholders, not completed functionality

### Outputs
Current state:
- dedicated Outputs tab exists
- output-side target toggles were renamed to user-facing labels:
  - `Send to Memory`
  - `Send to Author's Note`
  - `Send to Lorebook`
- a single shared `Wrapper` field exists
- Wrapper is editable now with the normal explicit edit/save/cancel workflow
- Outputs includes a short planning-only note

Important interpretation:
- Outputs is intentionally placeholder-oriented right now
- live output writing is still far-future work
- wrapper exists as a shared future output-structure field, not as a wired routing feature

### History
Current state:
- max history entries now has a user-facing config home under History
- history limit is a slider with typed entry
- changes do not immediately trim stored history
- first trim occurs on the next refresh

### Workspace header / operational controls
Current state:
- `Refresh`
- auto refresh checkbox
- auto refresh cadence numeric field
- cadence disabled while auto refresh is disabled

Auto-refresh specifics:
- cadence is a simple numeric field
- default `1`
- valid range `1` to `15`

### Sidebar structure
Sidebar is now intentionally structured as:
- **Status Info** — concise, read-only
- **Advanced** — technical/system controls
- **Diagnostics** — debug/state inspection

Advanced currently includes:
- temperature
- max tokens
- auto-refresh reset behavior checkbox
- guarded `Reset Panel State`

### Initialization / reset behavior
Important change from earlier testing behavior:
- script load no longer auto-clears storage and auto-refreshes secret info every page load
- the old test reset path now lives behind `Reset Panel State`
- `Reset Panel State` now has a confirmation guard

### User notification behavior
A visible toast/salute now fires when the background secret-info refresh starts, so users have a clear signal that the script is already working.

## Deliberate Non-Features or Deferred Areas
These remain intentionally deferred after `1.5.1` and should not be quietly pulled into `1.5.2` unless the user redirects scope:

- live apply / clear / resync into Memory
- live apply / clear / resync into Author's Note
- live apply / clear / resync into Lorebook
- receipt validation against live target content
- ownership-marker splicing into live targets
- per-target wrapper settings
- advanced output write modes
- broad routing/default-behavior architecture
- finished lorebook selector UX
- multi-character output expansion
- non-character generalized source handling
- broad compatibility scaffolding for older archived revisions
- rigid extractor / schema-style redesign

Very important boundary:
- `1.5.2` should focus on **input assembly and lorebook discovery rules**, not on live output routing

## Open Decisions and Known Risks
### Open decisions
- whether the 5-button top-row tab layout is the final navigation layout or needs follow-up tuning if it wraps/cramps awkwardly
- whether later source work should keep Inputs as simple combined options or evolve into more structured submodes
- how manual lorebook selection should be implemented as a custom selector when the time comes
- how auto-discover characters should interact with later lorebook-backed discovery rules once prompt assembly changes
- whether the future Inputs/Outputs structure stays flat or gets second-level navigation inside a tab

### Known risks
- the Inputs tab is intentionally unwired right now; future agents must not accidentally present it as already functional
- lorebook selection has no built-in picker in the scripting UI; a custom selector workflow is expected later
- current prompts are still engineered around characters; accidentally broadening source inputs too early can degrade output quality by pulling in non-character material
- the Outputs tab now has a real wrapper field even though output routing is not active; future agents must keep that distinction clear
- moving too quickly into output routing would skip the agreed `1.5.2` input-assembly phase
- if a future agent treats prompt text as config rather than content, the current UI model will get muddier again

## Next-Phase Intent
`1.5.2` should begin the **lorebook discovery rules and input-assembly rework**.

Strong interpretation of what `1.5.2` should focus on first:
1. **Define the input assembly seam**
   - rework how the script assembles and feeds input to the secret-info generation call
   - use the new Inputs tab as the visible home for that work

2. **Build the first source-model layer**
   - ordinary use remains auto-discover characters
   - first explicit lorebook-input direction remains manually selected lorebooks
   - keep it conservative and character-biased at first

3. **Use the right technical seam**
   - prefer `onBeforeContextBuild` / context-building work for source assembly
   - do not jump ahead to output injection

4. **Keep Outputs planning-only**
   - Outputs can remain a structural preview/planning surface while Inputs architecture matures
   - wrapper stays editable as a prepared config, not a live routing feature

5. **Preserve the `1.5.1` UI/config model**
   - no need to reopen the major tab/config placement decisions unless a concrete UX issue appears

## Recommended Next Read
The next chat should read these first:

1. `secret_info_panel_v1_5_1_6.naiscript`
2. `scratch/secret_info_panel_handoff_1_5_1_breakpoint.md`
3. this handoff
4. `live/current_version.md`
5. `docs/secret_info_panel_roadmap.md`
6. `docs/secret_info_panel_architecture.md`
7. `docs/secret_info_panel_progress_tracker.md`
