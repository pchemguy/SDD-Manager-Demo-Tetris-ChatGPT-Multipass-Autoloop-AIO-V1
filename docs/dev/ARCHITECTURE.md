# Architecture

The [project brief](PROJECT.md) defines a static single-player browser game. The system has three cooperating blocks: a deterministic game core, a browser controller and a DOM presentation. Their separation allows gameplay checks without a browser while retaining real browser verification for native input and rendering.

## Blocks and dependency direction

| Block | Responsibility | Dependencies and ownership |
| --- | --- | --- |
| Game core | Piece geometry, board state, movement, rotation, gravity, locking, row clearing, scoring and game lifecycle | Owns all gameplay state; receives explicit actions, elapsed timestamps and a supplied random source; imports no DOM or browser storage APIs. |
| Browser controller | Translate keyboard and clock events into ordered actions; manage one animation loop and restart lifecycle | Depends on the core and presentation interfaces; owns event listeners and clock scheduling, not board mutations. |
| DOM presentation | Render board, active piece, score, status, help and native restart control | Reads immutable snapshots; emits user intent through the controller; owns DOM nodes and styles, not gameplay rules. |

The composition entry constructs the random source and state, connects the view/controller and starts one loop. The core never calls the controller or renderer. Input handlers cannot mutate board cells directly. Rendering cannot advance game time or consume randomness.

## State and lifecycle

One session owns the settled board, active piece, score and running/game-over state. Actions produce a coherent updated snapshot; a lock/clear/score/spawn transaction is observable as one state transition. Settled cells and active geometry remain separate until lock. Restart discards the previous session state and elapsed scheduling debt while keeping exactly one listener set and loop.

The controller normalizes event time to a monotonic clock. The core advances due gravity events in order before applying a later action, making identical action/time/random sequences reproducible. Background-tab intervals are suspended and the clock origin is rebased on return, preventing a hidden-page catch-up burst.

## Design choices and tradeoffs

Use small TypeScript modules and data transformations rather than a framework, inheritance tree or plug-in system. A fixed 200-cell DOM grid supports CSS styling and native controls without a custom Canvas accessibility layer. The model owns collision rules once; rendering and input consume them through the session boundary. An injected random source and explicit time provide meaningful test seams without exposing production test controls to players.

## Verification and boundaries

Core fixtures establish geometry, wall/floor collisions, rotation, line clearing, scoring, top-out and clean restart. Integration checks connect real core components and control time/randomness. Browser checks exercise keyboard delivery, page-scroll prevention, rendered state and restart in Chromium and Firefox under the [central policy](../../BROWSER_DEV_POLICY.md). No network service is required for play. [DECOMPOSITION](DECOMPOSITION.md) defines logical units; [SPEC](SPEC.md) owns precise behavior.

## Landing and assistance ownership

The core owns a single landing projector used by hard drop and the ghost; independent renderer calculations are prohibited. The randomizer owns bag state. Rotation owns deterministic kick candidates. Session state owns the hold slot and once-per-piece hold eligibility. The view receives projected geometry and hold presentation data without consuming the queue.

Ground-contact state and its deadline belong to the core, using supplied timestamps rather than browser timers. The controller supplies wakeups and inputs; the model settles due events before accepting same-time actions. This prevents timing policy from diverging between keyboard, gravity and hard drop.

## Penetrator movement

The shared landing component selects the lowest valid fit beneath a candidate pose. Its normal and penetrator policies share board geometry but differ in traversal constraints. The session alone owns activation and bonus accounting; movement and projection read the active mode. Temporary overlap is representation state, never a board write. Lock uses the same strict non-overlap validator in both modes, keeping the board invariant independent of traversal.
