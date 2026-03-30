# Project Map

## Authoritative Live Script

- `live/secret_info_panel_live.naiscript`

This is the current authoritative working script file and the default starting point for future edits.

## Current Version Anchor

- current promoted script path: `live/secret_info_panel_live.naiscript`
- current promoted source artifact: `archive/code/secret_info_panel_v1_5_1_6.naiscript`
- current promoted working version: `1.5.1.6`
- current alignment date: `2026-03-29`

## Current Documentation Set

- `docs/README.md`
- `docs/secret_info_panel_architecture.md`
- `docs/secret_info_panel_roadmap.md`
- `docs/secret_info_panel_progress_tracker.md`
- `docs/secret_info_panel_minimap.md`
- `docs/changelog.md`

Interpretation rule:

- `docs/README.md` is the docs index and current docs-side source-of-truth guide
- the rest of the docs set should be read against the live script and current version note
- if a doc conflicts with `live/secret_info_panel_live.naiscript`, the live script wins

## Root-Level Guidance Files

- `README.md` - plain-language repository overview
- `SOURCE_KNOWLEDGE_GUIDE.md` - GPT-safe reading order and continuity guidance
- `WORKFLOW.md` - revision and promotion workflow
- `PROJECT_MAP.md` - quick structure and naming map

## Scratch / Temporary Context

- `scratch/` for temporary work files and transient handoff/context files
- `scratch/secret_info_panel_handoff_1_5_2_breakpoint.md` for the current active breakpoint handoff when needed

Older scratch handoffs and superseded context notes should be treated as archived history in `archive/docs/`, not as active scratch context.

Treat `scratch/` as recent working context, not stronger authority than `live/` or the current docs set.

## Archive Locations

- `archive/code/` for versioned historical script revisions
- `archive/docs/` for superseded or historical documentation
- `archive/data/` for archived data artifacts
- `archive/forks/` for fork or proposal variants that are not the mainline authoritative script

## Naming Convention

- standardized repository folders use lowercase names
- the live script uses the fixed canonical name `secret_info_panel_live.naiscript`
- versioned historical scripts remain version-labeled in `archive/code/`
- current docs in `docs/` use the stable `secret_info_panel_*` naming pattern where applicable
