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

**R3 — Baseline movement and locking.** Left/right translate one column; soft drop attempts one row; clockwise rotation follows R10. A failed lateral/rotation action is a no-op. Gravity attempts one row every 1000 ms of visible running game time. A failed downward attempt follows R12. Hard/soft-drop distance awards no score. The controller performs due events before a player action at the same timestamp; spawning schedules the next gravity event one full interval later.

**R4 — Atomic lock and score.** Lock writes exactly four previously free cells, removes all completely occupied rows simultaneously and compacts rows downward with empty rows inserted at the top. A single lock clears zero to four rows; award 0, 100, 300, 500 or 800 points respectively. Score accumulates until restart. Only after clearing/scoring does the next piece spawn. The view cannot observe a partially locked or partially cleared board. No levels or gravity-speed progression are required.

**R5 — Shape source.** Draw successive bags, each containing every shape exactly once. Shuffle a fresh bag with a uniform Fisher–Yates shuffle using the supplied random source. The next bag starts only after the current seven are consumed. A testable supplied random source must allow repeatable fixtures; rendering and failed actions consume no draws.

## Browser interaction and presentation

**R6 — Controls.** ArrowLeft/ArrowRight move, ArrowUp rotates, ArrowDown soft-drops, Space hard-drops, C holds and R restarts. Movement/soft-drop may use native key repeat; rotation, hard drop, hold and restart ignore repeat events. While gameplay owns keyboard focus, handled arrow keys prevent page scrolling. Keys originating from native interactive controls retain their ordinary operation; button activation invokes only its labeled action. Unhandled keys retain browser behavior.

**R7 — View and scheduling.** Display the board, distinguishable piece colors, score, running/game-over status, instructions and restart. Occupied cells remain legible on the background and active geometry follows the model snapshot. The layout keeps the board and controls usable without horizontal page scrolling at a 360 CSS-pixel viewport; pointer controls do not replace keyboard access. A hidden document suspends simulation; return rebases elapsed time and retains state, without an invisible gravity burst. A single animation loop owns updates; rendering cannot mutate gameplay or consume randomness.

**R8 — Delivery and verification.** Deliver static assets with no game-service dependency. Use TypeScript and a DOM/CSS view. Follow [BROWSER_DEV_POLICY.md](../../BROWSER_DEV_POLICY.md), prioritizing Chromium and including Firefox. Headless UI interaction/screenshots are sufficient. Browser provisioning and repeated restart evidence must identify isolated compatible binaries and state. Setup and application tests are implementation work, not evidence supplied by this specification.

## Acceptance scenarios

| Contract | Objective acceptance |
| --- | --- |
| R1 | All seven shapes have four distinct cells; four rotations restore orientation; wall/floor/overlap rejection preserves state. |
| R2 | Blocked spawn ends play without overwrites; repeated restarts clear board/score and leave one functioning input path. |
| R3 | At 999 ms a fresh piece has not fallen; at 1000 ms it attempts one row. Ground-contact and deadline fixtures follow R12; failed soft drop does not shorten its interval. |
| R4 | Fixtures for 0/1/2/3/4 simultaneous rows verify compaction and the corresponding score; cleared top cells are empty. |
| R5 | Controlled random fixtures produce expected spawn identities and no extra draws on render or rejected actions. |
| R6–R7 | Real keyboard movement/rotation/restart, focus handling, scroll prevention, small viewport and hidden-tab rebasing behave as specified in both target engines. |
| R8 | Built static assets execute the core play/game-over/restart flow in isolated Chromium and Firefox environments. |

## Enhanced movement and assistance

**R9 — Hold.** A fresh session has an empty hold slot. Hold is allowed once per spawned piece. With an empty slot, store the current shape and draw the next; with a populated slot, swap identities without consuming another shape. The incoming piece uses spawn orientation/origin and fresh per-piece timing. A blocked incoming spawn ends the game. Hold becomes available again only after a piece locks. Failed or disallowed hold changes no state or random draws. Show the held shape or an empty indicator.

**R10 — Wall kicks.** For a requested clockwise rotation, test the rotated pose with ordered origin offsets (0,0), (-1,0), (1,0), (-2,0), (2,0), (0,-1). Accept the first strict fit. If none fits, preserve the pose. O does not rotate or kick. This is a deterministic custom kick policy; no SRS compatibility is claimed.

**R11 — Landing, ghost and hard drop.** For a normal piece, the landing is the last strict-fitting pose reached by repeated one-row descent before the first blocked step. When visible, the ghost displays precisely that pose using a distinguishable outline without changing board state, time or random draws. Hard drop commits the same pose. A positive-distance hard drop establishes ground contact at its action timestamp. A zero-distance hard drop preserves an existing deadline. Ghost and hard drop do not project through obstructions for normal pieces.

**R12 — Full interval locking.** A piece becomes grounded when a further downward step is unavailable. Start a 1000 ms deadline at that transition, including grounding caused by lateral movement, rotation or hard drop. Grounded movement/rotation preserves the existing deadline if still grounded. Becoming airborne cancels it; the next grounding starts a fresh interval. Failed downward attempts cannot shorten a deadline. On expiry, lock once if still grounded. At exact expiry, the due lock precedes input; input then targets the resulting current piece. Hold starts fresh timing on its incoming piece. Spawning grounded starts a deadline immediately; a spawn collision remains game over. Hidden time does not consume deadlines. No reset occurs from repeated zero-distance drop.

| Contract | Objective acceptance |
| --- | --- |
| R5 | Each consecutive group of seven draws is a permutation; two successive bags may share their boundary shape. Restart begins fresh state. |
| R9 | Empty hold consumes one queued shape; swapping consumes none; second hold before lock is a no-op; incoming top-out is game over. |
| R10 | Fixtures reach the first fitting kick candidate, reject exhausted candidates and leave O unchanged. |
| R11 | Ghost equals hard-drop geometry across walls and cavities; neither projection nor rendering consumes randomness. |
| R12 | Ground at t=0: no lock at 999 ms, one lock at 1000 ms; grounded shifts preserve the deadline; airborne departure/recontact restarts it; hard drop and hidden-tab rebasing obey the same rule. |

## Conditional ghost visibility

**R13 — Midline visibility.** In dynamic mode, show the ghost if and only if every projected occupied cell has y ≥ 10. Any cell at y ≤ 9 hides it, including a projection straddling rows 9 and 10. Recompute after movement, rotation, descent, hold, spawn and board changes. Visibility does not alter the landing query or hard drop. A lower-half landing becomes visible immediately; game over has no active ghost.

Acceptance: project fixtures entirely above, straddling and entirely below the midline; assert hidden, hidden and visible respectively, then move between those cases and verify geometry remains identical to hard drop.
