# Source Knowledge Guide

This file is meant to be uploaded as project source knowledge for GPTs that have weak tooling, limited API access, and poor self-checking.

It is intentionally conservative. It tells the model where truth lives, what is only guidance, what is historical, and how to avoid overclaiming.

## Core Rule

When files disagree, use this priority order:

1. `live/secret_info_panel_live.naiscript`
2. `live/current_version.md`
3. `docs/changelog.md`
4. `docs/README.md`
5. `docs/secret_info_panel_minimap.md`
6. the rest of `docs/`
7. relevant files in `scratch/`
8. relevant files in `archive/`

The live script is the implementation truth.

The current version note is the promotion truth.

The changelog is the repository-history truth.

Everything else is supportive context.

## What Each Top-Level Area Means

### `live/`

This is the most important directory.

- `live/secret_info_panel_live.naiscript` is the current authoritative working script.
- `live/current_version.md` records which archived snapshot was promoted into `live/secret_info_panel_live.naiscript` and what version label should be treated as current.

If a GPT only has time to read two files, it should read these two.

### `docs/`

This is the current documentation set, but not every file in it is equally strong.

- `docs/README.md` explains the intended source-of-truth order.
- `docs/secret_info_panel_minimap.md` is a fast navigation map.
- `docs/changelog.md` records promotions and repo-level movement.
- `docs/secret_info_panel_architecture.md` explains implemented concepts and contracts.
- `docs/secret_info_panel_roadmap.md` explains future direction.
- `docs/secret_info_panel_progress_tracker.md` explains package/workstream status.

Important caution:

The docs set on `main` was aligned to the promoted `1.5.1.6` baseline on `2026-03-29`, but the live script still outranks the docs for claims about exact implemented behavior.

Use the docs for concepts, structure, and intent. Do not use them as stronger authority than the live files.

### `scratch/`

This directory contains temporary but intentionally shareable working context.

Treat it as lower authority than `live/` and `docs/`, but higher value than random guesswork.

Typical uses:

- recent handoff notes
- breakpoint summaries
- compact context for the next chat
- temporary planning documents

Current important scratch files:

- `scratch/secret_info_panel_handoff_1_5_2_breakpoint.md`

How to use `scratch/` safely:

- Use it to understand recent intent, next-step priorities, or chat-established framing.
- Do not let it override the live script for claims about what already exists in code.
- If `scratch/` conflicts with `live/`, interpret that as "future intent vs current implementation," not as proof that the repo is broken.
- If older handoff context is needed, check `archive/docs/` rather than assuming it still lives in `scratch/`.

### `archive/`

This is historical material, not the current source of truth.

Subdirectories:

- `archive/code/` = versioned historical script snapshots
- `archive/docs/` = superseded docs
- `archive/forks/` = side variants or non-mainline proposals

Important caution:

Do not assume that the numerically newest-looking snapshot in `archive/code/` is the current authoritative version.

A snapshot becomes authoritative only when:

1. it is promoted into `live/secret_info_panel_live.naiscript`, and
2. `live/current_version.md` records that promotion.

This matters because archived filenames may include suffixes like `a`, `b`, or `c`, and future non-ignored files may appear before a promotion is finalized.

## What Root-Level Files Are For

- `README.md` = plain-language repository overview
- `PROJECT_MAP.md` = structure and naming map
- `WORKFLOW.md` = promotion/storage workflow
- this file = GPT-safe navigation guide for weak-tooling environments

These files are useful for orientation, but they do not outrank `live/`.

## Safe Reading Strategy For a Limited GPT

### If asked "What is current?"

Read:

1. `live/current_version.md`
2. `live/secret_info_panel_live.naiscript`
3. `docs/changelog.md`

### If asked "What is implemented right now?"

Read:

1. `live/secret_info_panel_live.naiscript`
2. `docs/secret_info_panel_architecture.md`
3. `docs/README.md`

When architecture text conflicts with the live script, the live script wins.

### If asked "What should happen next?"

Read:

1. `scratch/secret_info_panel_handoff_1_5_2_breakpoint.md` if present
2. `docs/secret_info_panel_roadmap.md`
3. `docs/secret_info_panel_progress_tracker.md`
4. `docs/changelog.md`

Interpret this as "recent intent plus older roadmap direction," not as a single perfectly synchronized source. If older handoff context is needed, check `archive/docs/`.

### If asked "How did the repo get here?"

Read:

1. `docs/changelog.md`
2. `live/current_version.md`
3. relevant files in `archive/code/`

### If asked "What older revision should I inspect?"

Look in `archive/code/`, but choose deliberately.

- Use the version named in `live/current_version.md` if you need the promoted source artifact.
- Use adjacent versions only when a comparison is explicitly needed.
- Do not wander deeply through old snapshots unless the task is truly historical.

## How To Search `scratch/` Safely

When searching `scratch/`, prefer file names that suggest one of these patterns:

- `handoff`
- `breakpoint`
- `context`
- `roadmap`

Read the most specific and current-looking file first.

In this repo, a good default order is:

1. `scratch/secret_info_panel_handoff_1_5_2_breakpoint.md`
2. relevant archived handoff/context files in `archive/docs/` if more history is needed

Use `scratch/` to answer questions like:

- "What was the intended next phase?"
- "What did the last chat want to preserve?"
- "What framing should guide the next revision?"

Do not use `scratch/` alone to answer:

- "What is definitely implemented?"
- "What is definitely the current live version?"

For those questions, always return to `live/`.

## Versioning And Snapshot Rules

This repo uses a promoted-live plus archived-snapshots model.

The pattern is:

1. new work is based from `live/secret_info_panel_live.naiscript`
2. working versions may be saved as named snapshots in `archive/code/`
3. one snapshot is eventually promoted into `live/`
4. `live/current_version.md` records that promotion
5. `docs/changelog.md` records the repo-level change

Important caution:

The promoted artifact label and the script's internal self-reported version string may not always be perfectly identical. If that happens:

- treat `live/current_version.md` as the repo's promotion label
- treat `live/secret_info_panel_live.naiscript` as the actual code
- do not invent a reconciliation unless the files explicitly provide one

## What Not To Rely On

### Ignored or local-only material

Do not assume ignored paths exist or matter to repo truth.

Examples currently ignored by the repo:

- `data/`
- `AGENT.md`
- `Skills/`

These are not part of the reliable source-knowledge surface.

### Runtime artifacts

Do not expect logs, test exports, or story storage files to be present in the repo.

`data/` is intentionally ignored, so runtime traces are not a dependable knowledge source.

### Filename-only reasoning

Do not make strong claims from filename order alone.

Examples of bad reasoning:

- "This archived file has the highest version-looking suffix, so it must be current."
- "This scratch note is newer, so it overrides the live script."
- "This old roadmap line says `1.5.0.5`, so the live repo must still be on `1.5.0.5`."

## Good Behavior For A Weak GPT

- Prefer a small number of strong files over a large number of weak ones.
- Cite file paths when making important claims.
- Separate "implemented now" from "planned next."
- Separate "current live truth" from "historical archive."
- Separate "recent handoff intent" from "authoritative code."
- If uncertain, say exactly what is uncertain and which files disagree.

## Practical Shortcuts

If a GPT needs a fast answer with minimal risk:

- current code: `live/secret_info_panel_live.naiscript`
- current promoted version: `live/current_version.md`
- repo history: `docs/changelog.md`
- docs index: `docs/README.md`
- quick navigation: `docs/secret_info_panel_minimap.md`
- next-phase handoff: `scratch/secret_info_panel_handoff_1_5_2_breakpoint.md`
- historical versions: `archive/code/`

## Final Interpretation Rule

The safest overall behavior in this repo is:

1. trust `live/` for current reality
2. use `docs/` for explanation and roadmap
3. use `scratch/` for recent handoff intent
4. use `archive/` only for history or explicit comparison

If a model follows that rule, it will avoid most of the mistakes that a low-tooling GPT would otherwise make here.
