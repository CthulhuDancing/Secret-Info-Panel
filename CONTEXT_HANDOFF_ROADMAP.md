# Secret Info Panel — Context Handoff + Roadmap (Sanitized Memory)

> Purpose: This file is the compact handoff context for another GPT session that cannot easily index the repository. It captures goals, current state, constraints, and a path map to key files.

## 1) What the user is trying to accomplish

The project is a **NovelAI Storyteller userscript** (not a production SaaS app) that should:
- generate and maintain a private secret-information continuity block,
- keep same-turn rerolls stable,
- support safer/manual editing,
- evolve toward controlled routing/receipt visibility,
- remain practical and teachable for iterative learning.

Important constraint:
- Keep improvements **pragmatic and lightweight** (avoid enterprise sprawl).

## 2) Current implementation reality (as of this branch)

### Live script + versioning
- Live authoritative script: `live/secret_info_panel_live.naiscript`
- Version note: `live/current_version.md`
- Changelog: `docs/changelog.md`
- Archive snapshots: `archive/code/`

### Recent implemented changes (high level)
- Promoted live script to `1.5.0.5` metadata.
- Added routed receipt/ownership status fields and derivation helpers.
- Added diagnostics summary for routing receipts.
- Added debounced draft persistence for edit sessions (to reduce temp storage write noise).
- Added conceptual module section markers in the single-file script.

## 3) Design stance for next GPT

The next GPT should optimize for:
1. **Userscript practicality first** (single-file acceptable).
2. **Incremental refactor passes** over broad rewrites.
3. **Behavior safety** over architectural purity.
4. **High signal changes**: readability + correctness + obvious UX/perf wins.

Avoid:
- over-abstracting into framework-like structure,
- introducing heavy patterns that raise maintenance burden.

## 4) Recommended roadmap (implementation-focused)

### Phase A — Stabilize core orchestration readability
- Refactor `refreshSecretInfo` into small internal steps while preserving behavior.
- Keep state transitions explicit and auditable.
- Minimize duplicated save/sync calls where safe.

### Phase B — Tighten state + log flow
- Keep sanitization at write boundaries where possible.
- Reduce repeated full normalization/sorting of log entries on read paths.
- Preserve safety fallback for malformed imported state.

### Phase C — Improve edit-session ergonomics
- Keep debounce behavior for draft persistence.
- Ensure no stale timer writes during save/cancel/clear transitions.
- Add brief script-log traces around edit persistence events if needed for debugging.

### Phase D — Routing/receipt confidence pass
- Validate ownership/receipt derivation paths for edge cases (disabled targets, missing signatures, stale receipt errors).
- Keep route diagnostics concise and human-readable.

### Phase E — Optional quality/perf pass (small)
- Cache or reuse expensive derived values within a refresh run.
- Keep optimizations simple and locally testable.

## 5) File path map for connector-limited indexing

### Root
- `README.md` — repository purpose and folder conventions
- `WORKFLOW.md` — workflow notes
- `PROJECT_MAP.md` — project map
- `CONTEXT_HANDOFF_ROADMAP.md` — this handoff memory file

### Live authoritative artifacts
- `live/secret_info_panel_live.naiscript` — primary userscript
- `live/current_version.md` — promoted source/version marker

### Current docs
- `docs/changelog.md`
- `docs/secret_info_panel_roadmap.md`
- `docs/secret_info_panel_architecture.md`
- `docs/secret_info_panel_progress_tracker.md`
- `docs/secret_info_panel_1_5_minimap.md`

### Archive (history/reference)
- `archive/code/` — versioned script snapshots
- `archive/forks/` — fork/proposal variants
- `archive/docs/` — older archived docs
- `archive/data/` — archived logs/storage exports

### Data/runtime artifacts
- `data/logs/`
- `data/test_exports/`

## 6) How to reason about the main script quickly

In `live/secret_info_panel_live.naiscript`, look for conceptual sections in this order:
1. config/runtime constants,
2. state-store load/save and sanitization,
3. prompt engine build/parse,
4. diagnostics snapshot + summaries,
5. panel UI builder,
6. controller actions (refresh/edit/clear).

## 7) Bootstrap prompt template for the next GPT session

Copy/paste this prompt into the next chat:

```text
You are reviewing a NovelAI Storyteller userscript repository.

Start by reading these files in order:
1) CONTEXT_HANDOFF_ROADMAP.md
2) README.md
3) live/secret_info_panel_live.naiscript
4) docs/secret_info_panel_roadmap.md
5) docs/secret_info_panel_architecture.md
6) docs/secret_info_panel_progress_tracker.md
7) docs/changelog.md
8) live/current_version.md

Constraints:
- This is a practical userscript project, not a production microservice app.
- Prefer incremental, low-risk improvements.
- Keep recommendations concrete, path-aware, and prioritized by impact.

Task:
- Summarize current architecture and immediate risks.
- Propose a phased roadmap of implementation steps with expected benefits and risk level.
- Identify the top 3 code changes to implement next in `live/secret_info_panel_live.naiscript`.
- For each change, include exact function names and likely side effects to watch.
```

## 8) Notes for continuity

If this file and in-code behavior drift, prioritize the script behavior in:
- `live/secret_info_panel_live.naiscript`

Then update this handoff file to keep future sessions aligned.
