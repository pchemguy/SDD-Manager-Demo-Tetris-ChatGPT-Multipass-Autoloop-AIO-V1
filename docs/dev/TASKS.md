# Implementation tasks

The hierarchy derives from [PLAN](PLAN.md), contracts from [SPEC](SPEC.md) and expected paths from [layout](layout.md). Every task includes relevant tests, error cases and affected documentation. All implementation is pending. Review-task acceptance ends at verified report publication; the coordinator subsequently closes the review issue and milestone, integrates a completed phase and publishes its release under PLAN. Those later lifecycle effects are not prerequisites for completing their own review task.

## Phase 1 — Playable classic game

- [ ] Phase 1 — Playable classic game
    - [ ] Milestone 1.1 — Board and piece foundation
        - [ ] T-001 — Establish the isolated TypeScript test toolchain
            Scope: package.json, package-lock.json, tsconfig.json, vite.config.ts, vitest.config.ts, playwright.config.ts; configuration and developer setup documentation. Depends on: none. Contracts: R8.
            Evidence: Pin compatible local tooling and expose typecheck, unit, build and browser test commands. Follow the central browser policy; demonstrate isolated Chromium and Playwright-managed Firefox provisioning with representative launch/interaction evidence before dependent browser work. Do not copy policy text into project documents.
        - [ ] T-002 — Implement board and tetromino geometry
            Scope: src/core/geometry.ts and tests/unit/geometry.test.ts. Depends on: T-001. Contracts: R1.
            Evidence: Verify four-cell shapes, spawn offsets, rotation identity and strict bounds/overlap fixtures.
        - [ ] T-003 — Implement deterministic initial session and piece draws
            Scope: src/core/randomizer.ts, src/core/session.ts and focused unit tests. Depends on: T-002. Contracts: R2, R5.
            Evidence: Verify empty initialization, controlled shape draws and rejected-spawn behavior without browser dependencies.
        - [ ] T-004 — Review, test and report milestone 1.1
            Depends on: T-001, T-002, T-003. Scope: whole milestone implementation and collaborating boundaries.
            Evidence: separate code review and required focused/regression/browser checks; repair blockers; document findings and a TODO section in docs/dev/reports/phases/1/1.1.md.
    - [ ] Milestone 1.2 — Playable browser loop
        - [ ] T-005 — Implement core movement and gravity transitions
            Scope: src/core/session.ts and tests/unit/session.test.ts. Depends on: T-004. Contracts: R1, R3.
            Evidence: Use explicit timestamps to verify accepted/rejected movement, gravity cadence, normal locking and action ordering.
        - [ ] T-006 — Connect keyboard and clock lifecycle
            Scope: src/browser/controller.ts, src/main.ts and tests/integration/controller.test.ts. Depends on: T-005. Contracts: R6–R7.
            Evidence: Verify native-key mapping, one subscription/loop owner, hidden-page suspension and rebasing using controlled clocks.
        - [ ] T-007 — Render the playable board and native restart control
            Scope: src/ui/view.ts, src/styles.css, index.html and tests/e2e/classic.spec.ts. Depends on: T-006. Contracts: R6–R8.
            Evidence: Exercise visible falling/locking and instructions with actual keyboard delivery and no page scrolling in both engines.
        - [ ] T-008 — Review, test and report milestone 1.2
            Depends on: T-005, T-006, T-007. Scope: whole milestone implementation and collaborating boundaries.
            Evidence: separate code review and required focused/regression/browser checks; repair blockers; document findings and a TODO section in docs/dev/reports/phases/1/1.2.md.
    - [ ] Milestone 1.3 — Complete classic session
        - [ ] T-009 — Implement simultaneous line clearing and scoring
            Scope: src/core/session.ts and tests/unit/scoring.test.ts. Depends on: T-008. Contracts: R4.
            Evidence: Verify 0–4-row compaction and exact score awards using independent board fixtures.
        - [ ] T-010 — Complete game-over and clean restart behavior
            Scope: src/core/session.ts, src/browser/controller.ts, src/ui/view.ts and regression tests. Depends on: T-009. Contracts: R2, R6–R7.
            Evidence: Verify blocked spawn preserves occupied cells, gameplay is disabled at game over and repeated restart clears gameplay without listener multiplication.
        - [ ] T-011 — Verify and document the complete classic browser game
            Scope: tests/e2e/classic.spec.ts, tests/e2e/fixtures.ts, README.md and docs/user/controls.md. Depends on: T-010. Contracts: R1–R8.
            Evidence: Run unit, integration, typecheck, production build and isolated Chromium/Firefox flow; document actual commands, controls and static serving. Verify 360px layout and keyboard focus.
        - [ ] T-012 — Review, test and report milestone 1.3
            Depends on: T-009, T-010, T-011. Scope: whole milestone implementation and collaborating boundaries.
            Evidence: separate code review and required focused/regression/browser checks; repair blockers; document findings and a TODO section in docs/dev/reports/phases/1/1.3.md.
    - [ ] Milestone 1.4 — Phase review
        - [ ] T-013 — Review, test and report phase 1
            Depends on: every delivery milestone in Phase 1 being complete and closed; tasks T-004, T-008, T-012.
            Evidence: review cross-milestone code, verify phase exits and prior behavior, fix blockers and commit docs/dev/reports/phases/1/PHASE-REPORT.md with findings/TODOs and acceptance evidence for subsequent phase closure/integration/release.

## Phase 2 — Modern movement and landing

- [ ] Phase 2 — Modern movement and landing
    - [ ] Milestone 2.1 — Modern piece movement
        - [ ] T-014 — Implement seven-piece bag randomization
            Scope: src/core/randomizer.ts and tests/unit/randomizer.test.ts. Depends on: T-013. Contracts: R5.
            Evidence: Verify every consecutive bag is a seven-shape permutation, seeded replay and no sequence consumption on rejected actions or render.
        - [ ] T-015 — Implement deterministic wall kicks
            Scope: src/core/geometry.ts and tests/unit/geometry.test.ts. Depends on: T-014. Contracts: R10.
            Evidence: Verify ordered kick selection, walls/floor, exhaustion, O invariance and collision preservation.
        - [ ] T-016 — Implement shared landing projection and hard drop
            Scope: src/core/landing.ts, src/core/session.ts, src/browser/controller.ts and focused tests. Depends on: T-015. Contracts: R11.
            Evidence: Verify normal projection stops at the first obstruction, commits the same pose on Space and consumes no random draw.
        - [ ] T-017 — Review, test and report milestone 2.1
            Depends on: T-014, T-015, T-016. Scope: whole milestone implementation and collaborating boundaries.
            Evidence: separate code review and required focused/regression/browser checks; repair blockers; document findings and a TODO section in docs/dev/reports/phases/2/2.1.md.
    - [ ] Milestone 2.2 — Delayed landing
        - [ ] T-018 — Implement grounded deadlines and reset rules
            Scope: src/core/session.ts and tests/unit/locking.test.ts. Depends on: T-017. Contracts: R12.
            Evidence: Use exact fake-time boundaries, grounded shifts, airborne departure/recontact, zero-distance drop and spawn-grounding fixtures.
        - [ ] T-019 — Integrate hard drop with the session clock
            Scope: src/browser/controller.ts and tests/integration/controller.test.ts. Depends on: T-018. Contracts: R6, R11–R12.
            Evidence: Verify due events precede same-time input, hidden time is suspended and one lock/spawn transaction occurs.
        - [ ] T-020 — Verify and document modern movement in browsers
            Scope: tests/e2e/movement.spec.ts, README.md and docs/user/controls.md. Depends on: T-019. Contracts: R3, R5, R10–R12.
            Evidence: Run both target engines with actual rotation/drop keys, delayed landing and classic scoring/top-out/restart regressions.
        - [ ] T-021 — Review, test and report milestone 2.2
            Depends on: T-018, T-019, T-020. Scope: whole milestone implementation and collaborating boundaries.
            Evidence: separate code review and required focused/regression/browser checks; repair blockers; document findings and a TODO section in docs/dev/reports/phases/2/2.2.md.
    - [ ] Milestone 2.3 — Phase review
        - [ ] T-022 — Review, test and report phase 2
            Depends on: every delivery milestone in Phase 2 being complete and closed; tasks T-017, T-021.
            Evidence: review cross-milestone code, verify phase exits and prior behavior, fix blockers and commit docs/dev/reports/phases/2/PHASE-REPORT.md with findings/TODOs and acceptance evidence for subsequent phase closure/integration/release.

## Phase 3 — Landing and hold assistance

- [ ] Phase 3 — Landing and hold assistance
    - [ ] Milestone 3.1 — Landing assistance
        - [ ] T-023 — Expose ghost projection and dynamic visibility
            Scope: src/core/session.ts, src/core/landing.ts and tests/unit/landing.test.ts. Depends on: T-022. Contracts: R11, R13.
            Evidence: Verify canonical geometry without mutation and visibility for wholly upper, straddling rows 9/10 and wholly lower projections, including transitions.
        - [ ] T-024 — Render the dynamic landing ghost
            Scope: src/ui/view.ts, src/styles.css and tests/e2e/assistance.spec.ts. Depends on: T-023. Contracts: R11, R13.
            Evidence: Verify hide/show transitions at the midline and exact agreement with hard drop in both engines; rendering consumes no randomness.
        - [ ] T-025 — Review, test and report milestone 3.1
            Depends on: T-023, T-024. Scope: whole milestone implementation and collaborating boundaries.
            Evidence: separate code review and required focused/regression/browser checks; repair blockers; document findings and a TODO section in docs/dev/reports/phases/3/3.1.md.
    - [ ] Milestone 3.2 — Hold control
        - [ ] T-026 — Implement hold state and spawn semantics
            Scope: src/core/session.ts and tests/unit/hold.test.ts. Depends on: T-025. Contracts: R9.
            Evidence: Verify empty-slot draw, populated swap without a draw, second-hold rejection and incoming spawn collision.
        - [ ] T-027 — Integrate hold input and held-piece presentation
            Scope: src/browser/controller.ts, src/ui/view.ts and tests/e2e/assistance.spec.ts. Depends on: T-026. Contracts: R6, R9, R11–R12.
            Evidence: Verify C is edge-triggered, the slot updates from model state and the incoming piece/ghost/deadline are coherent.
        - [ ] T-028 — Review, test and report milestone 3.2
            Depends on: T-026, T-027. Scope: whole milestone implementation and collaborating boundaries.
            Evidence: separate code review and required focused/regression/browser checks; repair blockers; document findings and a TODO section in docs/dev/reports/phases/3/3.2.md.
    - [ ] Milestone 3.3 — Phase review
        - [ ] T-029 — Review, test and report phase 3
            Depends on: every delivery milestone in Phase 3 being complete and closed; tasks T-025, T-028.
            Evidence: review cross-milestone code, verify phase exits and prior behavior, fix blockers and commit docs/dev/reports/phases/3/PHASE-REPORT.md with findings/TODOs and acceptance evidence for subsequent phase closure/integration/release.

## Phase 4 — Penetrator ability

- [ ] Phase 4 — Penetrator ability
    - [ ] Milestone 4.1 — Penetrator traversal
        - [ ] T-030 — Implement the lowest valid penetrator landing query
            Scope: src/core/landing.ts and tests/unit/penetrator.test.ts. Depends on: T-029. Contracts: R14.
            Evidence: Use independent stacks/cavities to verify the deepest free fit below obstructions and normal-versus-penetrator projection behavior.
        - [ ] T-031 — Implement penetrator movement candidate validation
            Scope: src/core/geometry.ts, src/core/session.ts and tests/unit/penetrator.test.ts. Depends on: T-030. Contracts: R14.
            Evidence: Verify transient overlap is permitted inside bounds only while a legal landing remains at or below; reject movement/rotation with no eventual fit.
        - [ ] T-032 — Integrate penetrator descent and strict locking
            Scope: src/core/session.ts and tests/unit/locking.test.ts. Depends on: T-031. Contracts: R14, R16.
            Evidence: Verify row-by-row traversal through overlap, ground detection at the lowest legal fit and atomic lock without overwriting settled cells.
        - [ ] T-033 — Review, test and report milestone 4.1
            Depends on: T-030, T-031, T-032. Scope: whole milestone implementation and collaborating boundaries.
            Evidence: separate code review and required focused/regression/browser checks; repair blockers; document findings and a TODO section in docs/dev/reports/phases/4/4.1.md.
    - [ ] Milestone 4.2 — Activation and rewards
        - [ ] T-034 — Implement activation and per-piece lifetime
            Scope: src/core/session.ts and tests/unit/penetrator.test.ts. Depends on: T-033. Contracts: R15–R16.
            Evidence: Verify one bonus is spent only for a successful activation, repeat/no-bonus actions are no-ops, hold is blocked and activation ends at lock.
        - [ ] T-035 — Implement four-line bonus rewards and restart
            Scope: src/core/session.ts and tests/unit/scoring.test.ts. Depends on: T-034. Contracts: R15.
            Evidence: Verify exactly-four-line rewards, cap three, earning after a spend, ordinary score preservation and zero bonuses after restart.
        - [ ] T-036 — Connect the activation hotkey and landing assistance
            Scope: src/browser/controller.ts, src/core/session.ts and tests/integration/controller.test.ts. Depends on: T-035. Contracts: R6, R14–R16.
            Evidence: Verify P ignores repeats, ghost/drop share the penetrator result and activation preserves a grounded deadline or cancels it when airborne.
        - [ ] T-037 — Review, test and report milestone 4.2
            Depends on: T-034, T-035, T-036. Scope: whole milestone implementation and collaborating boundaries.
            Evidence: separate code review and required focused/regression/browser checks; repair blockers; document findings and a TODO section in docs/dev/reports/phases/4/4.2.md.
    - [ ] Milestone 4.3 — Penetrator presentation
        - [ ] T-038 — Render activated pieces and ghosts in white
            Scope: src/ui/view.ts, src/styles.css and focused rendering tests. Depends on: T-037. Contracts: R17.
            Evidence: Verify active penetrator cells are white, normal piece/ghost colors are nonwhite and rendering overlay does not erase settled data.
        - [ ] T-039 — Render the live bonus counter
            Scope: src/ui/view.ts and tests/e2e/penetrator.spec.ts. Depends on: T-038. Contracts: R15, R17.
            Evidence: Verify one symbol per bonus at 0–3, accessible count text and immediate updates on earning, spending and restart.
        - [ ] T-040 — Verify and document the full penetrator flow
            Scope: tests/e2e/penetrator.spec.ts, README.md and docs/user/controls.md. Depends on: T-039. Contracts: R9, R11–R17.
            Evidence: Run both engines through earned activation, through-stack hard drop, hidden/visible ghost, hold rejection, deadline lock and restart; preserve normal gameplay regressions.
        - [ ] T-041 — Review, test and report milestone 4.3
            Depends on: T-038, T-039, T-040. Scope: whole milestone implementation and collaborating boundaries.
            Evidence: separate code review and required focused/regression/browser checks; repair blockers; document findings and a TODO section in docs/dev/reports/phases/4/4.3.md.
    - [ ] Milestone 4.4 — Phase review
        - [ ] T-042 — Review, test and report phase 4
            Depends on: every delivery milestone in Phase 4 being complete and closed; tasks T-033, T-037, T-041.
            Evidence: review cross-milestone code, verify phase exits and prior behavior, fix blockers and commit docs/dev/reports/phases/4/PHASE-REPORT.md with findings/TODOs and acceptance evidence for subsequent phase closure/integration/release.
            Include docs/dev/reports/IMPLEMENTATION-REPORT.md with full task-list result and aggregated unresolved/deferred TODOs, options and provenance.
