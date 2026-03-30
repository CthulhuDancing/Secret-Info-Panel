# Secret Info Panel Documentation Index

This directory contains the current working documentation for the Secret Info Panel project.

## Current source of truth order

When files disagree, use them in this order:

1. `live/secret_info_panel_live.naiscript`
2. `live/current_version.md`
3. `docs/changelog.md`
4. this file
5. the rest of the current docs in this directory
6. relevant handoff material in `scratch/`

The live script is the implementation truth.

The current version note is the promotion truth.

The changelog records repository-level movement.

The docs in this directory explain the current baseline, the preserved contracts, and the intended next packet.

## Current authoritative docs

- `secret_info_panel_architecture.md` - implemented baseline, storage boundaries, UI roles, and preserved contracts for the live script
- `secret_info_panel_roadmap.md` - future-direction guardrails and packet sequencing from the current live baseline
- `secret_info_panel_progress_tracker.md` - package-level execution status and handoff-forward status board
- `secret_info_panel_minimap.md` - quick navigation map for the repo and docs set
- `changelog.md` - repository and promotion history

## Current baseline

- promoted live script: `live/secret_info_panel_live.naiscript`
- current promoted source artifact: `archive/code/secret_info_panel_v1_5_1_6.naiscript`
- current working version: `1.5.1.6`
- docs aligned date: `2026-03-29`

## Current phase position

The repo is currently in a **post-`1.5.1` UI/config-groundwork, pre-`1.5.2` implementation** state.

Interpret that as:

- the `1.5.1` packet is already landed in the live script
- the current UI surface split and config placement decisions are baseline, not open redesign questions
- `1.5.2` should begin the input-assembly and lorebook-discovery work
- live output routing remains deferred

## Current implementation highlights

The live `1.5.1.6` baseline already includes:

- canonical body vs derived display separation
- preserved raw diagnostic response layers
- tempStorage-backed edit-session handling
- a split workspace with `Secret Info`, `Prompting`, `Inputs`, `Outputs`, and `History` tabs
- separate label/body edit flows for Secret Info
- route-planner scaffolding and target toggles for `memory`, `authorsNote`, and `lorebook`
- an editable shared output wrapper field
- guarded destructive reset behavior instead of automatic reset on script load
- sidebar diagnostics for contracts, routing, prompt checks, and runtime state

## Current interpretation rule

Read the project this way:

- `live/secret_info_panel_live.naiscript` explains what exists now
- this docs set explains how to interpret that baseline
- `scratch/secret_info_panel_handoff_1_5_2_breakpoint.md` explains what should happen next
- the roadmap should be used as packet-order guidance, not as proof that deferred features already exist

## What this docs set should preserve

These themes should stay consistent across the docs:

- `canonicalBody` is the downstream source of truth
- `derivedDisplayText` is rebuilt from committed canonical body
- raw model output is diagnostic only
- unsaved drafts remain draft-only until explicit save
- routing and future output work must derive from committed canonical body
- the sidebar is the compact monitoring and diagnostics plane
- the script panel / workspace is the dense working and editing plane
- `Inputs` is the prepared home for `1.5.2` source assembly work
- `Outputs` is still planning-only
- narrow structural changes remain preferred over broad rewrites
