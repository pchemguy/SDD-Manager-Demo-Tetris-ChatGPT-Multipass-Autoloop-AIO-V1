# Component decomposition

[ARCHITECTURE](ARCHITECTURE.md) defines block dependencies. The components below refine those blocks; [SPEC](SPEC.md) is the canonical behavior owner. Interfaces describe collaboration rather than a mandatory class hierarchy.

| Component | Responsibility and exclusions | Provided/required boundary | Verification seam |
| --- | --- | --- | --- |
| C1 Geometry and landing | Own strict fit, rotation kick candidates and the canonical landing query. | Session requests rotated poses or landing; view consumes the returned projection. | Walls, cavities, kick exhaustion, maximal landing and projection/drop agreement. |
| C2 Randomizer | Own the seven-piece bag and injected unbiased shuffle draws. | Session consumes queued identifiers; restart creates a new bag; no rendering dependency. | Every bag is a permutation and fixtures reproduce a sequence. |
| C3 Session | Own gameplay state, activation flag, bonus count, hold eligibility and per-piece lock state. | Activation spends a bonus only when a legal eventual landing exists; mode lasts until lock; traversal reads C1. | Intermediate-overlap, legal-lock, rejected-activation, cap/reward and restart checks. |
| C4 Clock/input controller | Own event subscriptions, monotonic scheduling, keyboard intent and hidden-page suspension. | Maps native events to session actions, then sends snapshots to the view. | Fake-clock action ordering plus real native browser input. |
| C5 DOM view | Own fixed cell grid, score/status/help and restart UI. | Snapshot + view options → DOM update; user intent → controller callback. | Render fixtures and browser assertions; never infer locked cells from CSS. |
| C6 Settings | Own typed defaults, current settings and validated change events independently of game restart. | Controller consults toggles; spawn captures locking policies; native view controls receive values and callbacks. | Toggle timing, stored-hold retention and next-preview nonconsumption. |
| C7 Settings resolver | Own allowlisted decoders and per-setting URL/cookie/default precedence. | Raw strings → typed settings plus valid explicit URL values to save; no side effects. | Mixed sources, invalid values, duplicates, explicit defaults and omission fixtures. |
| C8 Browser settings adapter | Own cookie parsing/writes and URL query synchronization. | C6 changes and C7 resolution effects → cookie/History API attempts; failures reported nonfatally. | Real reload/reopen tests and isolated denied-cookie/history adapters. |

## Collaboration and ownership

The composition entry creates one session and connects C4 to C5. C4 advances due model events, applies an input and renders the resulting snapshot. C3 requests geometry from C1 and shape identifiers from C2. No dependency points back from C1/C2 into the session or browser. Session snapshots expose data needed to render without granting mutation access to settled cells.

Initialization and restart reset the model and clock together. Failed collision-dependent actions preserve the prior state. A game-over transition prevents further gameplay actions while preserving the final board until restart. Listener/loop ownership stays with C4; restarting creates neither another loop nor another listener set.

C1 projects from the current pose without changing state or randomness. C3 commits the projected pose for hard drop and exposes that same projection to C5 for the ghost. Grounded state/deadline belongs to C3; C4 owns no competing lock timeout. Hold transfers shape identity, not a pose or deadline.

C1 evaluates strict final fits even when penetrator traversal allows temporary overlap. C3 validates the activation candidate before spending a bonus. Lock never overwrites an occupied cell; clearing and reward accounting occur after a valid lock and before the next spawn. C5 layers the white active geometry over settled cells without altering their stored identities.

C6 survives C3 restart. Feature availability is consulted at action time; locking fields are copied at spawn. C5 owns settings/preview/hold/bonus DOM nodes, but their values come only from snapshots and C6. Key handling ignores native interactive controls.

C8 reads sources at bootstrap, C7 resolves them, C6 adopts the result and C8 saves only valid explicit URL values. On player changes, C6 updates first, then C8 attempts both persistence effects independently. C7 never reads document.cookie or location directly.
