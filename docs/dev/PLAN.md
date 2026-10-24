# Delivery plan

Deliver the complete behavior in [SPEC](SPEC.md) through bounded usable increments. Phase 1 is a playable classic game; later phases preserve its playable path while adding their named capability. [ARCHITECTURE](ARCHITECTURE.md) and [DECOMPOSITION](DECOMPOSITION.md) define logical ownership; layout assigns physical homes.

## Verification and integration strategy

Use pinned TypeScript/Vite tooling, Vitest for pure-core and controller integration checks, and Playwright for native browser flows. These are selected tools, not installed or verified capabilities. The first implementation task pins compatible versions and validates commands and isolated browser provisioning under [BROWSER_DEV_POLICY.md](../../BROWSER_DEV_POLICY.md). Do not derive browser requirements from globally available installations.

Every delivery task includes meaningful tests and affected documentation. Control time and randomness; use independently prepared board fixtures. Combine pure-rule tests with real cooperating components and a small set of end-to-end browser scenarios. Production test fixtures must not expose a player-facing debug console. Both target engines verify the integrated changed behavior; pure model checks need no browser.

Every delivery milestone ends with explicit implementation code review, relevant tests/regressions, blocker repair and a committed milestone report containing a TODO section. Each phase ends in a separate review milestone with exactly one phase code review/testing/report task, after all delivery milestones and their issues are complete/closed. The last phase review also produces the final implementation report and aggregated unresolved TODOs. Tests alone do not substitute for code review.

Complete phases integrate with an explicit two-parent merge, merged-state checks and verified publication to the default branch. Release each completed, verified and integrated phase using its version, tag and exact finalized release notes. Partial task ranges remain on their phase branch. Implementation and release execution require their own authorization; this plan supplies delivery strategy.

## Phases and milestones

### Phase 1 — Playable classic game

Outcome: A complete classic play, score, top-out and restart experience. Scope: R1–R8. Release: v0.1.0.

Scope rationale: All included mechanics are necessary to complete one usable game; independent assistance and configuration do not belong in this baseline.

Prerequisite: reviewed preparation and an authorized implementation range.

| Milestone | Integrated outcome / exit |
| --- | --- |
| 1.1 — Board and piece foundation | Establish repeatable geometry and spawn state plus an isolated test environment. Finish its code review, checks and report with no required blocker. |
| 1.2 — Playable browser loop | Demonstrate keyboard-controlled falling and locking pieces on the real board. Finish its code review, checks and report with no required blocker. |
| 1.3 — Complete classic session | Demonstrate scoring, top-out, restart and the built static application. Finish its code review, checks and report with no required blocker. |
| 1.4 — Phase review | Review the complete phase after delivery milestone closure; verify included contracts and prior-phase regressions, report findings/TODOs and confirm integration readiness. |

Decision evidence: demonstrate the integrated milestone behavior and inspect usability of its changed controls. Report evidence for the human to continue, amend, simplify or stop; do not infer a new scope or implement automatic steering.

## Risks and exit evidence

Timing/input ordering, top-out after a transaction, repeated sessions and cross-engine keyboard focus are primary risks. Reproducible model fixtures and real browser actions cover different failure classes. An unavailable required browser remains an explicit blocked check; proactively diagnose supported alternatives under the central policy before declaring a setup blocker.

Phase exits require scoped acceptance, relevant regressions, accurate user/setup documentation, a passing production build, implementation code review and evidence-backed reports. Native browser results must state actual versions, isolated locations and limitations. No runtime verification, package QC or release completion is claimed by this document.
