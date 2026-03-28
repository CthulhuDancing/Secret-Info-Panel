# Full Breakpoint Handoff

## Scope
This handoff closes out the `1.5.0.x` groundwork family and reframes the start of `1.5.1` around **UI stabilization and future-feature carveout** rather than another core state-contract refactor.

The `1.5.0.x` family should now be treated as the completed groundwork packet that established and preserved the active-use ownership model, dense over-input workspace, preview-only planning layer, and clean separation between canonical output, derived display, raw diagnostics, and draft state.

`1.5.1` should begin from that stable base and focus first on **fixing and simplifying the UI**, especially the relationship between the sidebar and the over-input workspace, while also making sure the UI layout deliberately reserves room for future features such as source selection, lorebook-related planning, and later routing growth.

## Source Basis and Confidence
- Authoritative basis: current project docs on `main` and the completed `1.5.0.x` family work as established in this chat
- Supporting basis: architecture, roadmap, progress tracker, and the finalized `1.5.0.x` UI/workflow decisions reached in this chat
- Inferred context: exact `1.5.1` sequencing and UI grouping emphasis, synthesized from the current family endpoint and user direction in this chat
- Confidence: high

## Current Baseline
Current working family endpoint: **`1.5.0.7c`**

Included at that endpoint:
- preserved canonical-output ownership cleanup from `1.5.0.6`
- over-input dense working surface with tabs
- sidebar reduced toward compact monitoring and control
- history moved out of the sidebar into the over-input workspace
- route planner kept preview-only
- `Planning` tab includes route toggles and a first source-selection surface
- `Prompting` combines prompt editing and preview concerns

## Stable Contracts and Rules
These are considered preserved family-level contracts and should remain intact in `1.5.1` unless explicitly changed on purpose:

- `canonicalBody` is the authoritative downstream source.
- `derivedDisplayText` is rebuilt from committed canonical body.
- `secretInfo` remains the visible stored display representation.
- raw model output remains diagnostic only.
- unsaved drafts remain draft-only until explicit save.
- secret-info editing remains body-only at the canonical edit layer.
- same-turn reroll protection remains intact.
- future routing must derive from committed canonical body, not raw output.
- routing/planning remains preview-only until live apply is intentionally started later.
- clean-slate initialization on script load remains intended current behavior.
- sidebar remains the compact monitoring/control plane.
- over-input workspace remains the dense working/editing plane.
- narrow structural changes remain preferred over broad rewrites.

## Authoritative References
Treat these as the primary references for continuing into `1.5.1`:

1. `live/secret_info_panel_live.naiscript`
2. `live/current_version.md`
3. `docs/secret_info_panel_architecture.md`
4. `docs/secret_info_panel_roadmap.md`
5. `docs/secret_info_panel_progress_tracker.md`
6. this handoff

If the next chat needs to reason about what is already protected versus what is now open for change, use the architecture + roadmap pairing first, then confirm present details from the live script.

## Current State
The project is now past the authority-cleanup stage and the first over-input workspace migration stage.

The important current interpretation is:
- the **state model is no longer the primary unfinished area**
- the **UI surface model is now the main unfinished area**

The current workspace arrangement is usable, but `1.5.1` should assume that the UI still needs refinement in order to become the real long-term operating surface.

The main relevant outcomes from `1.5.0.x` are:
- active-use authority cleanup landed
- canonical vs display vs raw vs draft separation is established
- same-turn reroll protection remained intact through the family
- route planning exists but remains preview-only
- dense editing now lives over the input instead of depending on the sidebar
- tab organization has started to become workflow-based
- a first `Sources` concern is now acknowledged in the UI rather than being left implicit

That means `1.5.1` should not reopen foundational ownership work unless a true regression is found.

Instead, `1.5.1` should treat the UI as the next major design surface to stabilize and future-proof.

## Deliberate Non-Features or Deferred Areas
These remain intentionally deferred at the `1.5.1` starting point:

- live apply / clear / resync into Memory
- live apply / clear / resync into Author's Note
- live apply / clear / resync into Lorebook
- receipt validation against live target content
- ownership-marker splicing into live targets
- full lorebook discovery system
- multi-character output expansion
- heavy compatibility scaffolding for old archived revisions
- broad routing engine abstractions
- rigid extractor / schema-style redesign

Important interpretation:
`1.5.1` may **prepare** the UI for future lorebook-related work, but it should **not** skip ahead into finished lorebook routing implementation.

## Open Decisions and Known Risks
### Open decisions
- whether the over-input workspace header should become the permanent home for shared actions such as `Refresh`, `Clear Saved Block`, and auto-refresh state
- final tab organization for the dense workspace after the initial `Prompting` / `Planning` consolidation
- how much future-feature carveout should be visible now versus merely reserved in the layout
- how to represent source strategy in the UI once it grows beyond a simple lorebook toggle / fallback concept
- how to reserve space for future lorebook-related planning without prematurely implying live lorebook mutation
- whether planning-related controls should eventually split into `Planning` and `Sources`, or remain merged until feature depth justifies a separate tab

### Known risks
- reintroducing sidebar/workspace duplication after the family already worked to reduce it
- letting UI rearrangement accidentally disturb canonical commit semantics
- letting future-feature placeholders become misleading pseudo-features
- overbuilding lorebook UI before the actual source/routing model is ready
- creating a workspace layout that feels finished for current features but leaves no clean place for the next planned surfaces

## Next-Phase Intent
`1.5.1` should be grounded in **UI correction and future-feature carveout**.

That means the next phase should prioritize:

1. **Fix and stabilize the UI layout**
   - make the over-input workspace clearly primary
   - keep the sidebar compact and non-duplicative
   - move shared actions into the workspace header when that improves usability across tabs
   - reduce friction caused by tab switching for core actions

2. **Make the UI deliberately future-aware**
   - preserve space for source selection
   - preserve a clear future lane for lorebook-related planning
   - preserve room for later target planning growth without implying live apply exists now
   - avoid packing the current UI so tightly around current-only features that future additions become awkward

3. **Refine the tab model around workflow, not internal implementation layers**
   - editing tab
   - prompting tab
   - planning/sources tab
   - history tab
   - shared actions above the tab body rather than trapped inside one tab or the sidebar

4. **Keep `1.5.1` post-groundwork, not pre-groundwork**
   - do not reopen the family’s already-settled authority contracts unless needed
   - use the preserved state model as the stable base beneath the UI work
   - only begin deeper feature growth once the UI is shaped to receive it cleanly

A strong interpretation for the start of `1.5.1` is:

> finish making the UI feel like the real long-term operating surface, and ensure it visibly leaves room for future sources/lorebook-related work, before expanding into deeper routing or source behavior.

## Recommended Next Read
The next chat should read these first:

1. `live/current_version.md`
2. `live/secret_info_panel_live.naiscript`
3. `docs/secret_info_panel_architecture.md`
4. `docs/secret_info_panel_roadmap.md`
5. `docs/secret_info_panel_progress_tracker.md`
6. this handoff
