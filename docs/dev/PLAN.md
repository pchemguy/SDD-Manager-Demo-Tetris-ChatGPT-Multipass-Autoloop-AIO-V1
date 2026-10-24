# Delivery plan

Deliver the complete behavior in [SPEC](SPEC.md) through bounded usable increments. Phase 1 is a playable classic game; later phases preserve its playable path while adding their named capability. [ARCHITECTURE](ARCHITECTURE.md) and [DECOMPOSITION](DECOMPOSITION.md) define logical ownership; [layout](layout.md) assigns physical homes.

## Verification and integration strategy

Use pinned TypeScript/Vite tooling, Vitest for pure-core and controller integration checks, and Playwright for native browser flows. These are selected tools, not installed or verified capabilities. The first implementation task pins compatible versions and validates commands and isolated browser provisioning under [BROWSER_DEV_POLICY.md](../../BROWSER_DEV_POLICY.md). Do not derive browser requirements from globally available installations.

Every delivery task includes meaningful tests and affected documentation. Control time and randomness; use independently prepared board fixtures. Combine pure-rule tests with real cooperating components and a small set of end-to-end browser scenarios. Production test fixtures must not expose a player-facing debug console. Both target engines verify the integrated changed behavior; pure model checks need no browser.

Every delivery milestone ends with explicit implementation code review, relevant tests/regressions, blocker repair and a committed milestone report containing a TODO section. Each phase ends in a separate review milestone with exactly one phase code review/testing/report task, after all delivery milestones and their issues are complete/closed. The last phase review also produces the final implementation report and aggregated unresolved TODOs. Tests alone do not substitute for code review.

Complete phases integrate with an explicit two-parent merge, merged-state checks and verified publication to the default branch. Release each completed, verified and integrated phase using its version, tag and exact finalized release notes. Partial task ranges remain on their phase branch. Implementation requires a selected range. Once implementation is authorized, the standing user instruction authorizes a release after each completed, verified and integrated phase; no separate release approval is required.

## Phases and milestones

### Phase 1 — Playable classic game

Outcome: A complete classic play, score, top-out and restart experience. Scope: R1–R8 classic subset: strict unshifted rotation, independent shape draws and failed-downward locking; the bag, kick and delayed-lock refinements are delivered in Phase 2. Release: v0.1.0.

Scope rationale: All included mechanics are necessary to complete one usable game; independent assistance and configuration do not belong in this baseline.

Prerequisite: reviewed preparation and an authorized implementation range.

| Milestone | Integrated outcome / exit |
| --- | --- |
| 1.1 — Board and piece foundation | Establish repeatable geometry and spawn state plus an isolated test environment. Finish its code review, checks and report with no required blocker. |
| 1.2 — Playable browser loop | Demonstrate keyboard-controlled falling and locking pieces on the real board. Finish its code review, checks and report with no required blocker. |
| 1.3 — Complete classic session | Demonstrate scoring, top-out, restart and the built static application. Finish its code review, checks and report with no required blocker. |
| 1.4 — Phase review | Review the complete phase after delivery milestone closure; verify included contracts and prior-phase regressions, report findings/TODOs and confirm integration readiness. |

Decision evidence: demonstrate the integrated milestone behavior and inspect usability of its changed controls. Report evidence for the human to continue, amend, simplify or stop; do not infer a new scope or implement automatic steering.

### Phase 2 — Modern movement and landing

Outcome: A playable game with bag randomization, wall kicks, hard drop and consistent delayed locking. Scope: R5 bag behavior, R10–R12 movement/landing and the associated R3/R6 refinements. Release: v0.2.0.

Scope rationale: Shared landing and timing contracts form one coherent movement increment. Hold and visual assistance are independent and remain in Phase 3.

Prerequisite: Phase 1 completed, reviewed, integrated and published; preserve its acceptance and acquire an authorized range.

| Milestone | Integrated outcome / exit |
| --- | --- |
| 2.1 — Modern piece movement | Demonstrate bag draws, deterministic wall kicks and hard-drop landing in the running game. Finish its code review, checks and report with no required blocker. |
| 2.2 — Delayed landing | Demonstrate deadline-based grounding after gravity, movement and hard drop. Finish its code review, checks and report with no required blocker. |
| 2.3 — Phase review | Review the complete phase after delivery milestone closure; verify included contracts and prior-phase regressions, report findings/TODOs and confirm integration readiness. |

Decision evidence: demonstrate the integrated milestone behavior and inspect usability of its changed controls. Report evidence for the human to continue, amend, simplify or stop; do not infer a new scope or implement automatic steering.

### Phase 3 — Landing and hold assistance

Outcome: A playable game with a faithful landing ghost and a once-per-piece hold slot. Scope: R9 hold, R11 ghost and R13 dynamic midline visibility. Release: v0.3.0.

Scope rationale: These are optional player aids on the stable movement/landing baseline. Separate model/view checks cover them without expanding the first enhanced movement phase.

Prerequisite: Phase 2 completed, reviewed, integrated and published; preserve its acceptance and acquire an authorized range.

| Milestone | Integrated outcome / exit |
| --- | --- |
| 3.1 — Landing assistance | Demonstrate that the visible ghost agrees with the actual hard drop. Finish its code review, checks and report with no required blocker. |
| 3.2 — Hold control | Demonstrate empty hold, swap, hold eligibility and hold top-out in real play. Finish its code review, checks and report with no required blocker. |
| 3.3 — Phase review | Review the complete phase after delivery milestone closure; verify included contracts and prior-phase regressions, report findings/TODOs and confirm integration readiness. |

Decision evidence: demonstrate the integrated milestone behavior and inspect usability of its changed controls. Report evidence for the human to continue, amend, simplify or stop; do not infer a new scope or implement automatic steering.

### Phase 4 — Penetrator ability

Outcome: An earnable penetrator ability with safe through-stack traversal, faithful landing assistance and visible bonus accounting. Scope: R14–R17 with R9/R11–R13 interactions. Release: v0.4.0.

Scope rationale: Traversal, activation, rewards and presentation form one usable ability; presenting activation without its safe landing or earned resource would be incomplete. Existing normal gameplay remains unchanged.

Prerequisite: Phase 3 completed, reviewed, integrated and published; preserve its acceptance and acquire an authorized range.

| Milestone | Integrated outcome / exit |
| --- | --- |
| 4.1 — Penetrator traversal | Establish safe mode-aware traversal and strict legal final placement. Finish its code review, checks and report with no required blocker. |
| 4.2 — Activation and rewards | Demonstrate earned, spent and capped activations with correct hold/timer interactions. Finish its code review, checks and report with no required blocker. |
| 4.3 — Penetrator presentation | Demonstrate white activated pieces and a synchronized accessible bonus indicator. Finish its code review, checks and report with no required blocker. |
| 4.4 — Phase review | Review the complete phase after delivery milestone closure; verify included contracts and prior-phase regressions, report findings/TODOs and confirm integration readiness. |

Decision evidence: demonstrate the integrated milestone behavior and inspect usability of its changed controls. Report evidence for the human to continue, amend, simplify or stop; do not infer a new scope or implement automatic steering.

### Phase 5 — Session configuration

Outcome: Native session settings and next preview that can be changed safely during play. Scope: R18–R20 and their gameplay interactions. Release: v0.5.0.

Scope rationale: Typed settings and native controls form one useful configuration increment. Persistence is independent and belongs to Phase 6; the page-session behavior is complete without it.

Prerequisite: Phase 4 completed, reviewed, integrated and published; preserve its acceptance and acquire an authorized range.

| Milestone | Integrated outcome / exit |
| --- | --- |
| 5.1 — Session settings | Demonstrate six labeled controls, defaults and the read-only next preview. Finish its code review, checks and report with no required blocker. |
| 5.2 — Live policy integration | Demonstrate immediate visibility/toggle effects and next-piece locking-policy changes. Finish its code review, checks and report with no required blocker. |
| 5.3 — Phase review | Review the complete phase after delivery milestone closure; verify included contracts and prior-phase regressions, report findings/TODOs and confirm integration readiness. |

Decision evidence: demonstrate the integrated milestone behavior and inspect usability of its changed controls. Report evidence for the human to continue, amend, simplify or stop; do not infer a new scope or implement automatic steering.

### Phase 6 — Persistent configuration

Outcome: Settings restore from cookies and shared URLs with independent precedence and reliable user-change synchronization. Scope: R21–R24 and persistent R19 lifetime. Release: v0.6.0.

Scope rationale: Persistence is an independent adapter/resolution increment on the usable session configuration baseline. It adds no gameplay rules, server or saved game state. Two delivery milestones separate restoration from write synchronization.

Prerequisite: Phase 5 completed, reviewed, integrated and published; preserve its acceptance and acquire an authorized range.

| Milestone | Integrated outcome / exit |
| --- | --- |
| 6.1 — Settings resolution | Demonstrate per-key restoration before the first piece spawns. Finish its code review, checks and report with no required blocker. |
| 6.2 — Settings synchronization | Demonstrate user-change cookie/URL updates and reload/reopen behavior. Finish its code review, checks and report with no required blocker. |
| 6.3 — Phase review | Review the complete phase after delivery milestone closure; verify included contracts and prior-phase regressions, report findings/TODOs and confirm integration readiness. |

Decision evidence: demonstrate the integrated milestone behavior and inspect usability of its changed controls. Report evidence for the human to continue, amend, simplify or stop; do not infer a new scope or implement automatic steering.

## Risks and exit evidence

Timing/input ordering, top-out after a transaction, repeated sessions and cross-engine keyboard focus are primary risks. Reproducible model fixtures and real browser actions cover different failure classes. An unavailable required browser remains an explicit blocked check; proactively diagnose supported alternatives under the central policy before declaring a setup blocker.

Phase exits require scoped acceptance, relevant regressions, accurate user/setup documentation, a passing production build, implementation code review and evidence-backed reports. Native browser results must state actual versions, isolated locations and limitations. No runtime verification, package QC or release completion is claimed by this document.

## Intermediate delivery acceptance

Phase 1 retains its simple classic behavior: strict unshifted rotations, independent uniform shape draws and locking on a failed downward attempt. Phase 2 replaces those interim rules with the target R5/R10/R12 guarantees and adds hard drop. Phase 3 supplies hold and ghost presentation. Phase 5 settings survive in-page restart but use defaults after reload and do not read or write URL/cookie settings. Phase 6 replaces that interim lifetime with R19/R21–R24 persistence, including updating the earlier settings-suite reload expectation while retaining its other assertions. The full target remains specified in SPEC; earlier phase exits verify their included subset and do not claim the later guarantees.

These separate increments preserve a useful game at every release. Repeated two-milestone phases are retained for distinct coherent outcomes, not numerical quotas; merging player assistance into the movement phase would delay verification of the independently useful drop/timing path.
