# Secret Info Panel Documentation Index

This directory contains the current working documentation for the Secret Info Panel project.

## Current source of truth order

When documents disagree, use them in this order:

1. `live/secret_info_panel_live.naiscript`
2. `live/current_version.md`
3. `docs/changelog.md`
4. the current docs in this directory

The live script is the implementation truth. The version note is the promoted-release truth. The changelog records repository-level movement. The docs in this directory explain the current baseline, the remaining roadmap, and the active work packages.

## Current authoritative docs

- `secret_info_panel_architecture.md` - implemented baseline for the current live script
- `secret_info_panel_roadmap.md` - future work not yet fully implemented
- `secret_info_panel_progress_tracker.md` - package-level execution status and handoff notes
- `secret_info_panel_minimap.md` - quick navigation guide for the repository and docs set
- `changelog.md` - repository and promotion history

## Current baseline

- promoted live script: `live/secret_info_panel_live.naiscript`
- current working version: `1.5.0.6`
- date aligned: `2026-03-28`

## Interpretation rule

The project is no longer in the older `1.4.21` pre-contract state. The live `1.5.0.6` script already includes:

- canonical body vs derived display separation
- preserved raw diagnostic response layers
- routed-state scaffolding with target adapters, ownership markers, and receipt status placeholders
- tempStorage-backed edit-session handling for dense field edits
- diagnostics that expose route adapter and receipt summaries

Roadmap and progress notes should treat those items as present baseline work, not as wholly future concepts.
