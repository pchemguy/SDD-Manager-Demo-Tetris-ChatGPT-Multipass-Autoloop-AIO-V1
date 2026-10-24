# Component decomposition

[ARCHITECTURE](ARCHITECTURE.md) defines block dependencies. The components below refine those blocks; [SPEC](SPEC.md) is the canonical behavior owner. Interfaces describe collaboration rather than a mandatory class hierarchy.

| Component | Responsibility and exclusions | Provided/required boundary | Verification seam |
| --- | --- | --- | --- |
| C1 Geometry and landing | Own strict fit, rotation kick candidates and the canonical landing query. | Session requests rotated poses or landing; view consumes the returned projection. | Walls, cavities, kick exhaustion, maximal landing and projection/drop agreement. |
| C2 Randomizer | Own the seven-piece bag and injected unbiased shuffle draws. | Session consumes queued identifiers; restart creates a new bag; no rendering dependency. | Every bag is a permutation and fixtures reproduce a sequence. |
| C3 Session | Own board, active pose, score, hold slot, hold eligibility and grounded deadline. | Serialize movement/hold/drop/tick/restart; lock/clear/score/spawn remains atomic. | Fake-time deadline boundaries, hold swaps, top-out and rejected-action invariants. |
| C4 Clock/input controller | Own event subscriptions, monotonic scheduling, keyboard intent and hidden-page suspension. | Maps native events to session actions, then sends snapshots to the view. | Fake-clock action ordering plus real native browser input. |
| C5 DOM view | Own fixed cell grid, score/status/help and restart UI. | Snapshot + view options → DOM update; user intent → controller callback. | Render fixtures and browser assertions; never infer locked cells from CSS. |

## Collaboration and ownership

The composition entry creates one session and connects C4 to C5. C4 advances due model events, applies an input and renders the resulting snapshot. C3 requests geometry from C1 and shape identifiers from C2. No dependency points back from C1/C2 into the session or browser. Session snapshots expose data needed to render without granting mutation access to settled cells.

Initialization and restart reset the model and clock together. Failed collision-dependent actions preserve the prior state. A game-over transition prevents further gameplay actions while preserving the final board until restart. Listener/loop ownership stays with C4; restarting creates neither another loop nor another listener set.

C1 projects from the current pose without changing state or randomness. C3 commits the projected pose for hard drop and exposes that same projection to C5 for the ghost. Grounded state/deadline belongs to C3; C4 owns no competing lock timeout. Hold transfers shape identity, not a pose or deadline.
