# Full Breakpoint Handoff

## Scope
This handoff captures the end-of-family breakpoint after the `1.5.0.x` groundwork packet, with the project now positioned to begin `1.5.1` work. It covers the stable baseline contracts inherited from the live script and aligned docs on `main`, plus the net-new intent and implemented direction from this chat across `1.5.0.6` through `1.5.0.7c`.

## Source Basis and Confidence
- Authoritative basis: `live/current_version.md`; `docs/secret_info_panel_architecture.md`; `docs/secret_info_panel_roadmap.md`; `docs/secret_info_panel_progress_tracker.md`; the fetched `main` live script baseline in `live/secret_info_panel_live.naiscript`
- Supporting basis: the chat-established revision sequence and requested scope boundaries for `1.5.0.6` and the final `1.5.0.7x` UI/planner refinements
- Inferred context: the exact `1.5.0.7c` file contents are treated as current working output from this chat, but are not yet reflected in the fetched `main` docs or live script on GitHub
- Confidence: medium-high

## Current Baseline
The authoritative documented repo baseline on `main` is still the `1.5.0.6` checkpoint in `live/current_version.md`, while the project-working continuation from this chat should be treated as the end-of-family `1.5.0.7c` snapshot prepared for the next phase. The stable interpretation is:

- `1.5.0.5` remained the documented live architectural baseline on `main`
- `1.5.0.6` completed Workstream A: commit semantics and authority cleanup
- `1.5.0.7x` completed the final narrow groundwork pass for the family by moving dense work into the over-input script-panel workspace, reducing sidebar duplication, and keeping routing preview-only
- the intended handoff start point for `1.5.1` is the `1.5.0.7c` working snapshot, not older branch assumptions

## Stable Contracts and Rules
The following rules are now the important preserved core of the `1.5.0.x` family and should carry forward into `1.5.1` unless explicitly changed:

1. **Canonical output ownership remains explicit**
   - committed canonical body is the authoritative downstream source
   - derived display text is rebuilt from canonical body
   - visible `Secret information:` label is display-only and not editable truth

2. **Raw output remains diagnostic**
   - raw model response and raw response body are preserved as diagnostic layers
   - future routing must not derive from raw output

3. **Draft state remains draft-only**
   - edit-session drafts live in temp state
   - drafts become authoritative only on explicit save
   - save/cancel semantics remain explicit

4. **Same-turn reroll protection remains non-negotiable**
   - `storyTurnCounter`, `promptSeedTurnCounter`, and `promptSeedSecretInfo` continue to anchor rerolls to stable turn-backed seed state
   - same-turn refreshes must not recursively contaminate themselves

5. **Clean-slate initialization remains intended baseline behavior**
   - script load / runtime reinitialization still resets the panel’s own saved state and rebuilds a fresh baseline
   - do not reinterpret this as a persistence bug unless project direction changes explicitly later

6. **Routing remains preview-only**
   - routed-state scaffolding is real baseline work and should be extended, not discarded
   - planner state may represent targets, desired signatures, and preferences
   - no live Memory / Author’s Note / Lorebook apply engine has been started yet

7. **UI roles are now intentionally split**
   - sidebar = compact control / monitoring / diagnostics surface
   - over-input script panel = dense working / editing / planning / history surface

8. **Narrow structural changes remain preferred**
   - avoid speculative abstractions, broad compatibility layers, or large rewrites
   - preserve the readable project shape and teachable revision-to-revision continuity

## Authoritative References
Read these first before starting `1.5.1`:

1. `live/current_version.md`
2. `docs/secret_info_panel_architecture.md`
3. `docs/secret_info_panel_roadmap.md`
4. `docs/secret_info_panel_progress_tracker.md`
5. the `1.5.0.7c` working snapshot from this chat

Interpretation order for present continuation:
- use repo and docs on `main` for stable contracts and documented sequencing
- use the `1.5.0.7c` snapshot from this chat as the practical working code basis for the next revision
- do not fall back to older pre-contract assumptions

## Current State
At the end of this chat, the project has been intentionally steered to a family-complete state for `1.5.0.x`.

### What `1.5.0.6` accomplished
`1.5.0.6` was the Workstream A cleanup pass.

It tightened:
- explicit canonical commit behavior
- explicit separation of draft state, canonical body, derived display text, and raw diagnostics
- central commit pathways for save/generate/clear behavior
- reconstruction from committed canonical body instead of trusting stored display text as an authority layer

This was validated in-chat as preserving:
- canonical vs raw separation
- draft-only edit semantics until save
- same-turn reroll anchoring
- clean-slate initialization behavior

### What the `1.5.0.7x` pass accomplished
The final `1.5.0.7` family refinements focused on UI roles and preview-only planning without undoing the authority cleanup.

The family endpoint established in `1.5.0.7c` is:
- over-input workspace uses tabs
- tabs were condensed into:
  - `Secret Info`
  - `Prompting`
  - `Planning`
  - `History`
- `Prompts` + `Preview` were merged into `Prompting`
- `Routes` was renamed to `Planning`
- a `Sources` section was added under `Planning`
- source behavior currently uses the existing `lorebookOnlyCharacterDetection` setting:
  - enabled = lorebook-only
  - disabled = prefer lorebooks with story-context fallback
- duplicate current secret-info display was removed from the sidebar
- per-target text-body previews were removed from the route checkboxes
- history logs were moved out of the sidebar into the dedicated over-input `History` panel so the full stored history can be reviewed there

### What remained intentionally unchanged during the UI pass
The UI/workspace changes were kept narrow and were not intended to alter:
- canonical ownership rules
- raw diagnostic handling
- reroll protection
- clean-slate initialize behavior
- preview-only routing boundary

## Deliberate Non-Features or Deferred Areas
The following remain intentionally deferred at the family breakpoint and should not be treated as already-landed work:

- live apply / clear / resync into Memory
- live apply / clear / resync into Author’s Note
- live apply / clear / resync into Lorebook
- ownership-marker splicing into live targets
- receipt validation against real target content
- full lorebook / source discovery system
- multi-character output expansion
- broad config/schema framework
- broad migration / compatibility layers for older archived revisions
- prompt-engineering expansion tied to live routing

## Open Decisions and Known Risks
### Open decisions
1. **What exactly opens `1.5.1`**
   The project is now at the boundary where the next revision can move beyond groundwork-only family cleanup. The main pending decision is which narrowly-scoped next phase becomes the opening objective for `1.5.1`.

2. **Source model evolution**
   A basic `Sources` surface now exists conceptually inside `Planning`, but the deeper source model is still unresolved:
   - explicit lorebook-only
   - lorebook-preferred with story fallback
   - broader hybrid source logic
   - eventual per-character or inspectable source attribution

3. **Preview-planner depth**
   The planner remains preview-only. The next decision is how much richer planned/current/dirty target inspection should become before any live apply work starts.

4. **Small config layer shape**
   The roadmap still calls for injection-ready config tightening. The exact shape for output mode, target enablement, route behavior, and editor-surface preferences remains open.

### Known risks
1. **Do not let UI reorganization weaken state ownership**
   Any `1.5.1` work must preserve the distinction between raw generation, draft state, committed canonical body, and derived display text.

2. **Do not let source controls silently become live routing**
   The new `Sources` surface should remain planning/config-only unless live apply is intentionally opened later.

3. **Do not accidentally weaken reroll protection**
   Prompt seed logic remains one of the most important cross-cutting contracts.

4. **Do not accidentally reinterpret initialization behavior**
   Reset-on-script-load is still intentional and should not be broken casually while evolving UI or planner state.

5. **History is full stored history, not unbounded history**
   The dedicated `History` panel is meant to show the full currently stored set. Storage remains bounded by `maxHistoryEntries`.

## Next-Phase Intent
The next phase should begin `1.5.1` from the completed `1.5.0.x` groundwork packet rather than treating the project as still in the same cleanup family.

The safest reading is:
- `1.5.0.x` completed the hardening and UI-role split needed before the next phase
- `1.5.1` should now take one narrow forward step beyond that family while preserving all family-end contracts

The best candidate starting direction for `1.5.1` is a narrow continuation of preview-only planning/config/source work rather than an immediate jump to live target mutation.

That means `1.5.1` should likely focus on one of these, not all at once:
- tightening the planner state shape
- making the `Sources` surface more explicit and legible
- formalizing the small config layer for later routing behavior
- improving target plan inspection without applying anything live yet

## Recommended Next Read
For the next chat, read in this order:

1. `docs/secret_info_panel_roadmap.md`
2. `docs/secret_info_panel_architecture.md`
3. `docs/secret_info_panel_progress_tracker.md`
4. the `1.5.0.7c` working snapshot from this chat
5. this handoff
