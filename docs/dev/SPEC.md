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

**R3 — Baseline movement and locking.** Left/right translate one column; soft drop attempts one row; clockwise rotation follows R10. A failed lateral/rotation action is a no-op. Gravity attempts one row every 1000 ms of visible running game time. A failed downward attempt follows the piece's captured general policy (R20); interval locking uses R12. Hard/soft-drop distance awards no score. The controller performs due events before a player action at the same timestamp; spawning schedules the next gravity event one full interval later.

**R4 — Atomic lock and score.** Lock writes exactly four previously free cells, removes all completely occupied rows simultaneously and compacts rows downward with empty rows inserted at the top. A single lock clears zero to four rows; award 0, 100, 300, 500 or 800 points respectively. Score accumulates until restart. Only after clearing/scoring does the next piece spawn. The view cannot observe a partially locked or partially cleared board. No levels or gravity-speed progression are required.

**R5 — Shape source.** Draw successive bags, each containing every shape exactly once. Shuffle a fresh bag with a uniform Fisher–Yates shuffle using the supplied random source. The next bag starts only after the current seven are consumed. A testable supplied random source must allow repeatable fixtures; rendering and failed actions consume no draws.

## Browser interaction and presentation

**R6 — Controls.** ArrowLeft/ArrowRight move, ArrowUp rotates, ArrowDown soft-drops, Space hard-drops, C holds, P activates a penetrator and R restarts. Movement/soft-drop may use native key repeat; rotation, hard drop, hold, activation and restart ignore repeat events. While gameplay owns keyboard focus, handled arrow keys prevent page scrolling. Keys originating from native interactive controls retain their ordinary operation; button activation invokes only its labeled action. Unhandled keys retain browser behavior.

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

**R11 — Landing, ghost and hard drop.** For a normal piece, the landing is the last strict-fitting pose reached by repeated one-row descent before the first blocked step. When visible, the ghost displays precisely that pose using a distinguishable outline without changing board state, time or random draws. Hard drop commits the same pose. A positive-distance hard drop establishes ground contact at its action timestamp. With interval locking and hard drop following the general policy, a zero-distance hard drop preserves an existing deadline; immediate locking follows R20. Ghost and hard drop do not project through obstructions for normal pieces.

**R12 — Full interval locking.** Apply this timer contract to interval-policy pieces except when immediate hard-drop locking applies under R20. A piece becomes grounded when a further downward step is unavailable. Start a 1000 ms deadline at that transition, including grounding caused by lateral movement, rotation or hard drop. Grounded movement/rotation preserves the existing deadline if still grounded. Becoming airborne cancels it; the next grounding starts a fresh interval. Failed downward attempts cannot shorten a deadline. On expiry, lock once if still grounded. At exact expiry, the due lock precedes input; input then targets the resulting current piece. Hold starts fresh timing on its incoming piece. Spawning grounded starts a deadline immediately; a spawn collision remains game over. Hidden time does not consume deadlines. No reset occurs from repeated zero-distance drop.

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

## Penetrator contracts

**R14 — Traversal and eventual fit.** A penetrator's final landing is the strict-fitting pose with maximum origin y among all origins at or below its current origin, for its current orientation and column. Search through intermediate obstructions; unlike a normal drop, the first occupied row does not stop this query. A penetrator candidate is admissible only if all its cells are in bounds and at least one strict-fitting landing exists at or below it. Temporary overlap with settled cells is allowed for an admissible candidate and never destroys or writes those cells. Apply this candidate validator to lateral moves and the ordered R10 rotation kick trials. Rejected candidates preserve all state.

During gravity/soft drop, move one row at a time through admissible intermediate positions until the deepest legal landing. At that landing the piece is grounded even when a lower in-board row would overlap, because no legal landing remains below. Hard drop commits the deepest legal landing immediately; the ghost previews exactly that pose and R13 evaluates those projected occupied cells. A blocked intervening stack does not truncate the penetrator projection. Normal pieces retain R11's first-obstruction rule.

**R15 — Activation and bonus accounting.** Initialize with zero bonuses. After a valid lock clears exactly four lines, add one bonus up to a maximum of three; keep ordinary scoring unchanged. No other clear count earns a bonus. On P, if running, not already activated, bonus count ≥ 1 and the activated pose is admissible under R14, spend exactly one and mark this piece as a penetrator. Rejected activation, key repeat and activation at game over change nothing. One activation lasts through this piece's movement until lock; the next piece is regular. An activated penetrator cannot be held; C is a no-op without changing the slot, queue, bonus count or deadline. Restart clears all bonuses and activation state.

**R16 — Activation, timing and locking.** On activation, recompute grounded status under R14. If airborne, cancel the prior grounded deadline. If still grounded, preserve its existing deadline rather than granting extra time; a new grounding follows the captured general policy (R20). Penetrator hard drop follows the piece's captured R20 policies. In the interval/follow case, a positive-distance drop starts full grounding delay and a zero-distance drop preserves the deadline. Grounded expiry locks once using strict bounds/non-overlap validation. Lock may never overwrite settled cells, including after overlapping traversal. Clearing, score, bonus reward and next spawn are one atomic transaction.

**R17 — Visual identity.** Render every activated piece cell with white fill (#ffffff) on a contrasting board; use a white outline for its ghost when visible. Regular pieces and their ghosts use nonwhite shape colors. Settled cells retain their shape identity and regular color when the activation ends at lock. Show a labeled side-panel bonus counter with exactly one lightning-bolt symbol per stored bonus, plus an accessible numeric count including zero. Update it on earn, spend and restart in the same rendered snapshot. An active white overlay may obscure an underlying cell temporarily but cannot delete its model data.

| Contract | Objective acceptance |
| --- | --- |
| R14 | A board with an intervening occupied band and a lower fitting cavity yields the lower cavity for penetrator ghost/drop and the upper obstruction for a normal piece. A sideways/rotated candidate without any valid fit at or below is rejected without mutation. |
| R14, R16 | Traverse overlapping intermediate positions and verify settled cell identities are unchanged; lock only at a strict fit, exactly once. |
| R15 | Exercise zero/one/three bonus counts, successful spend, repeated activation, non-four clears, capped four-line rewards, hold rejection, game over and restart. |
| R16 | Activate a grounded piece with a lower landing: the old deadline is canceled. Activate one still grounded: its deadline is preserved. Ghost visibility and hard drop agree under both timing cases. |
| R17 | Inspect active/ghost/settled colors and exact symbol/count updates in both target engines, including immediately after bonus earn/spend. |

## Session settings

**R18 — Controls, values and defaults.** A side panel exposes six native labeled controls. Display their selected values and a short explanation that locking changes apply to the next piece. The ghost modes control visibility only; all use the same canonical projection, including penetrators. Always-show displays a projection whenever an active piece exists; no ghost is rendered at game over.

| Setting | Allowed values | Default |
| --- | --- | --- |
| Ghost visibility | hide, dynamic, show | dynamic |
| Hard drop | off, on | on |
| Next-piece preview | off, on | on |
| Hard-drop locking | follow general, immediate | follow general |
| General locking | failed downward attempt, full interval | full interval |
| Hold | off, on | on |

The next preview shows exactly the shape that the next queue-consuming spawn will use. Prepare queue state during session/randomizer transactions, never while rendering or toggling preview. Empty hold consumes the previewed shape; a populated hold swap does not advance it. Hide the preview without changing the sequence when off. Settings controls and restart remain keyboard accessible without their arrow/Space/C/P/R events being interpreted as gameplay.

**R19 — Immediate effects and session lifetime.** Visibility, hard-drop availability, next preview and hold availability apply at the next rendered/action snapshot without restarting or advancing the game. A disabled drop/hold action is a no-op; recognized gameplay keys still prevent scrolling while the game owns focus. Disabling hold preserves the stored shape and the once-per-piece eligibility flag. Re-enabling cannot grant another hold on the same piece. Penetrator hold remains prohibited even when the hold setting is on. Settings changes do not consume random draws, alter bonuses or reset timers. Settings survive R/restart in the current page. Reload/reopening resolves them through R21–R22; R23 synchronizes subsequent player changes. Persist these settings only, not board, score, active/held pieces or bonuses.

**R20 — Lock-policy capture and combinations.** Every spawned piece, including an incoming hold/restart piece, captures both selected locking policies. Later setting changes update the panel but preserve that piece's captured policies and any pending deadline. Interval policy follows R12. Failed-attempt policy creates no grounded deadline: lock on a failed downward attempt by gravity or soft drop, while grounded movement/rotation remains legal until that attempt.

When hard drop is enabled, first commit the normal or penetrator canonical landing. Captured immediate hard-drop locking locks once at that timestamp, including a zero-distance drop. Captured follow-general with interval policy uses R12's full interval after positive-distance grounding and preserves an existing deadline on a zero-distance drop. Captured follow-general with failed-attempt policy treats the blocking end of the drop as the failed downward attempt and locks once immediately. Hard-drop availability is live; it does not change a captured policy or an already pending deadline.

| Contract | Objective acceptance |
| --- | --- |
| R18 | All six controls start at documented defaults and are independently operable through native focus. Preview is identical to the next queue-consuming spawn and is unaffected by render/toggle calls. |
| R19 | Toggle ghost modes while crossing the midline; disable/re-enable hold before/after use; disable drop while grounded; verify no board, timer, bonus or sequence mutation beyond the selected effect. Restart retains choices; reload follows independent URL/cookie/default resolution. |
| R20 | Test both general policies × both hard-drop policies, including positive/zero-distance drops and penetrators. Change policies at 500 ms of a 1000 ms grounded deadline: the existing deadline is unchanged and the next spawned/held piece captures the new selections. |

## Configuration persistence

C6 owns current settings, C7 resolves raw values and C8 adapts cookies/URL updates as defined in DECOMPOSITION. The application is served from an HTTP(S) origin; file-URL persistence is outside the supported environment. Cookie continuity requires the same origin and an unexpired cookie. Gameplay still starts fresh on reload.

**R21 — Wire values and independent precedence.** Recognize only these exact case-sensitive query names and decoded tokens. Boolean tokens map 0 → off and 1 → on. Cookie values use the same tokens. Whitespace, case variants, empty strings and all other tokens are invalid. Decode cookie values independently; malformed encoding invalidates that cookie only. Unknown query parameters/cookies do not affect settings.

| Setting | URL parameter | Cookie | Valid decoded tokens |
| --- | --- | --- | --- |
| Ghost visibility | ghost | tetris_ghost | hide, dynamic, show |
| Hard drop | hardDrop | tetris_hardDrop | 0, 1 |
| Next preview | next | tetris_next | 0, 1 |
| Hard-drop locking | hardDropLock | tetris_hardDropLock | follow, immediate |
| General locking | lock | tetris_lock | failed, interval |
| Hold | hold | tetris_hold | 0, 1 |

For each row independently: a valid explicit URL token wins; otherwise use its valid cookie; otherwise use its R18 default. An invalid explicit URL value does not suppress a valid cookie. For duplicate query names use the first occurrence, including falling back when that first value is invalid; do not search later duplicates for a valid value. For duplicate same-name cookies use the first matching value in document.cookie order. A problem with one key cannot reset another key.

**R22 — Startup and cookie lifetime.** Resolve before the first piece is spawned or controls rendered. Save each valid explicitly supplied URL token into its corresponding cookie, including tokens equal to the default or to the existing cookie. Omitted or invalid URL parameters do not overwrite saved cookie values. Merely applying a cookie/default at startup does not rewrite other cookies or insert URL parameters. Save cookies as percent-encoded allowlisted tokens with Path=/, SameSite=Lax and Max-Age=31536000; add Secure on HTTPS. These are JavaScript-readable settings cookies; no other persistent storage is used.

**R23 — Player-change synchronization.** Whenever a player changes a control, adopt its typed value in page memory, attempt to save that setting's cookie and set its query parameter to the selected wire token, including an explicit default. Update the existing address with history.replaceState; preserve path, unrelated parameters and the URL fragment, collapse duplicate occurrences of the affected setting and avoid navigation or a new history entry. Do not regenerate the whole settings query from defaults or touch unrelated cookies/setting parameters. Continue to apply R19/R20 timing: saved/selected locking policies may differ from the current piece's captured policy until another piece spawns.

**R24 — Failure and validation isolation.** Cookie reads/writes and History API operations may fail or be denied. Resolve unavailable cookies as absent; never crash or prevent a game from starting for persistence failure. A player change remains effective in memory; attempt cookie and URL effects independently so failure of either does not suppress the other. Show a nonblocking settings-save status when a failure is detected; do not claim successful saving after a detected failure. No persistence failure changes gameplay state, randomness, bonuses, hold eligibility or current deadlines. Invalid supplied values fall back without requiring user intervention.

| Contract | Objective acceptance |
| --- | --- |
| R21 | For every setting, exercise valid URL over conflicting cookie, valid cookie with omitted URL, default with neither source, invalid URL over valid cookie and invalid cookie without valid URL. Mix those cases across different keys in one initialization. |
| R21 | Verify empty/case/space/unknown tokens, malformed cookie encoding, duplicate query first-value semantics and duplicate cookie handling without cross-key reset. |
| R22 | A valid default-valued URL overwrites a conflicting saved cookie; an omitted or invalid parameter preserves its cookie. Inspect all six cookie names/attributes and first-piece policies after reload. |
| R23 | Change every control to nondefault and default values; verify the corresponding cookie and query token, preserved unrelated parameters/fragment, no reload/history growth and unchanged current-piece lock policy. |
| R22–R23 | Reload and reopen on the same origin, then open a copied explicit settings URL with conflicting cookies; independently selected values and explicit URL cookie saves must match the resolver. |
| R24 | Deny cookie access, silently reject cookie writes where observable and reject replaceState through isolated adapter fixtures; game and memory settings still work, the other persistence effect is attempted and detected failure status is nonblocking. |
