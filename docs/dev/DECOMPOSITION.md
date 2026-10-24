# Component decomposition

[ARCHITECTURE](ARCHITECTURE.md) defines block dependencies. The components below refine those blocks; SPEC is the canonical behavior owner. Interfaces describe collaboration rather than a mandatory class hierarchy.

| Component | Responsibility and exclusions | Provided/required boundary | Verification seam |
| --- | --- | --- | --- |
| C1 Geometry | Tetromino orientations, board bounds and occupied-cell tests. No score, time or DOM state. | Pose and board inputs → cells, fit result and rotated candidate; reusable by session rules. | Boundary fixtures: side walls, floor, O invariance and four-rotation identity. |
| C2 Randomizer | Draw shape identifiers from an injected random source; own sequence state. | Session requests one spawn identifier; view never draws. | Controlled draws and restart isolation. |
| C3 Session | Own settled cells, active pose, score and running/game-over state; serialize actions and lock transactions. | Initial state + action + time + randomizer → snapshot; no renderer callbacks. | Real geometry/randomizer collaboration and atomic clear/spawn checks. |
| C4 Clock/input controller | Own event subscriptions, monotonic scheduling, keyboard intent and hidden-page suspension. | Maps native events to session actions, then sends snapshots to the view. | Fake-clock action ordering plus real native browser input. |
| C5 DOM view | Own fixed cell grid, score/status/help and restart UI. | Snapshot + view options → DOM update; user intent → controller callback. | Render fixtures and browser assertions; never infer locked cells from CSS. |

## Collaboration and ownership

The composition entry creates one session and connects C4 to C5. C4 advances due model events, applies an input and renders the resulting snapshot. C3 requests geometry from C1 and shape identifiers from C2. No dependency points back from C1/C2 into the session or browser. Session snapshots expose data needed to render without granting mutation access to settled cells.

Initialization and restart reset the model and clock together. Failed collision-dependent actions preserve the prior state. A game-over transition prevents further gameplay actions while preserving the final board until restart. Listener/loop ownership stays with C4; restarting creates neither another loop nor another listener set.
