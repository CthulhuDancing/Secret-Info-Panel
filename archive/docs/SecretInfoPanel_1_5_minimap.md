# Secret Info Panel 1.5 Mini-Map

- **Lock the canonical contract**: keep raw output diagnostic, keep editable output canonical, keep display text derived, and add only the minimum routed-state needed for sync/ownership.
- **Add the routing foundation, not full routing sprawl**: define route adapters, route receipts, ownership markers, and route-status concepts before trying to fully wire every target.
- **Preserve the 1.4 architecture**: do not collapse prompt fields, current output, generated state, and prompt preview back into mixed state.
- **Keep reroll/turn stability sacred**: any new routed or sync behavior must not interfere with same-turn seed protection or clean turn ordering.
- **Prefer small durable storage additions**: add narrow routing/sync state only where needed; avoid broad schema expansion.
- **Treat manual edits as first-class source truth**: downstream routing should consume the edited canonical output, not the raw model response.
- **Do not make 1.5 a UI-first version**: keep the sidebar as control/status/summary; deeper editor migration stays secondary to the data/routing foundation.
- **Do not make 1.5 a prompt-rewrite version**: prompt changes stay out unless a structural need appears while defining the routing contract.
- **Leave room for diagnostics skeletons**: only enough sync/status visibility to support the new contract, not a full diagnostics expansion yet.
- **Favor sturdy, simple outputs**: no rigid extractor turn, no clever over-modeling, no fragile process layers. Keep it soft, lightweight, and easy to reason about.
