# System specification

This document defines the observable contracts for the [project](PROJECT.md). Responsibilities are allocated in [DECOMPOSITION](DECOMPOSITION.md): C1 owns geometry, C2 randomness, C3 gameplay, C4 browser actions/time and C5 rendering. Requirements describe the intended application; no implementation is claimed.

## Board, pieces and time

**R1 — Geometry.** The visible board has 10 columns and 20 rows, indexed x=0…9 and y=0…19 downward. A pose consists of shape, orientation and an origin. Spawn uses orientation zero with origin (3,0). The occupied offsets are:

| Shape | Orientation-zero offsets |
| --- | --- |
| I | (0,1), (1,1), (2,1), (3,1) |
| O | (1,0), (2,0), (1,1), (2,1) |
| T | (1,0), (0,1), (1,1), (2,1) |
| J | (0,0), (0,1), (1,1), (2,1) |
| L | (2,0), (0,1), (1,1), (2,1) |
| S | (1,0), (2,0), (0,1), (1,1) |
| Z | (0,0), (1,0), (1,1), (2,1) |

Clockwise rotation transforms offsets around (1,1) for J/L/S/T/Z and (1.5,1.5) for I using (dx,dy) → (-dy,dx). O is invariant. All accepted active and settled cells lie inside the board. Strict fit requires no overlap with settled cells.

**R2 — Session lifecycle.** Initial load and restart create an empty board, score zero and a fresh shape sequence, then spawn one piece. A rejected normal spawn enters game over; it does not overwrite occupied cells. Game over ignores gameplay actions and preserves the last board/score. Restart is available through R and a labeled native button and begins a clean session without reloading or duplicating subscriptions.

**R3 — Baseline movement and locking.** Left/right translate one column; soft drop attempts one row; clockwise rotation accepts the unshifted rotated pose only when it strictly fits. A failed lateral/rotation action is a no-op. Gravity attempts one row every 1000 ms of visible running game time. A failed downward attempt locks the current piece. Hard/soft-drop distance awards no score. The controller performs due events before a player action at the same timestamp; spawning schedules the next gravity event one full interval later.

**R4 — Atomic lock and score.** Lock writes exactly four previously free cells, removes all completely occupied rows simultaneously and compacts rows downward with empty rows inserted at the top. A single lock clears zero to four rows; award 0, 100, 300, 500 or 800 points respectively. Score accumulates until restart. Only after clearing/scoring does the next piece spawn. The view cannot observe a partially locked or partially cleared board. No levels or gravity-speed progression are required.

**R5 — Shape source.** The initial shape and each following spawn draw independently with equal probability from the seven identifiers. A testable supplied random source must allow repeatable fixtures; rendering and failed actions consume no draws.

## Browser interaction and presentation

**R6 — Controls.** ArrowLeft/ArrowRight move, ArrowUp rotates, ArrowDown soft-drops and R restarts. Movement/soft-drop may use native key repeat; rotation and restart ignore repeat events. While gameplay owns keyboard focus, handled arrow keys prevent page scrolling. Keys originating from native interactive controls retain their ordinary operation; button activation invokes only its labeled action. Unhandled keys retain browser behavior.

**R7 — View and scheduling.** Display the board, distinguishable piece colors, score, running/game-over status, instructions and restart. Occupied cells remain legible on the background and active geometry follows the model snapshot. The layout keeps the board and controls usable without horizontal page scrolling at a 360 CSS-pixel viewport; pointer controls do not replace keyboard access. A hidden document suspends simulation; return rebases elapsed time and retains state, without an invisible gravity burst. A single animation loop owns updates; rendering cannot mutate gameplay or consume randomness.

**R8 — Delivery and verification.** Deliver static assets with no game-service dependency. Use TypeScript and a DOM/CSS view. Follow [BROWSER_DEV_POLICY.md](../../BROWSER_DEV_POLICY.md), prioritizing Chromium and including Firefox. Headless UI interaction/screenshots are sufficient. Browser provisioning and repeated restart evidence must identify isolated compatible binaries and state. Setup and application tests are implementation work, not evidence supplied by this specification.

## Acceptance scenarios

| Contract | Objective acceptance |
| --- | --- |
| R1 | All seven shapes have four distinct cells; four rotations restore orientation; wall/floor/overlap rejection preserves state. |
| R2 | Blocked spawn ends play without overwrites; repeated restarts clear board/score and leave one functioning input path. |
| R3 | At 999 ms a fresh piece has not fallen; at 1000 ms it attempts one row. A blocked downward attempt locks exactly once. |
| R4 | Fixtures for 0/1/2/3/4 simultaneous rows verify compaction and the corresponding score; cleared top cells are empty. |
| R5 | Controlled random fixtures produce expected spawn identities and no extra draws on render or rejected actions. |
| R6–R7 | Real keyboard movement/rotation/restart, focus handling, scroll prevention, small viewport and hidden-tab rebasing behave as specified in both target engines. |
| R8 | Built static assets execute the core play/game-over/restart flow in isolated Chromium and Firefox environments. |
