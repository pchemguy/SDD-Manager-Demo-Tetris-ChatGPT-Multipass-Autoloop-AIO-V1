# Implementation tasks

The hierarchy derives from [PLAN](PLAN.md), contracts from [SPEC](SPEC.md) and expected paths from [layout](layout.md). Every task includes relevant tests, error cases and affected documentation. All implementation is pending.

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
            Evidence: separate code review and required focused/regression/browser checks; repair blockers; document findings and a TODO section in docs/dev/reports/phases/1/1.1.md; publish and reconcile constituent issue/milestone closure.
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
            Evidence: separate code review and required focused/regression/browser checks; repair blockers; document findings and a TODO section in docs/dev/reports/phases/1/1.2.md; publish and reconcile constituent issue/milestone closure.
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
            Evidence: separate code review and required focused/regression/browser checks; repair blockers; document findings and a TODO section in docs/dev/reports/phases/1/1.3.md; publish and reconcile constituent issue/milestone closure.
    - [ ] Milestone 1.4 — Phase review
        - [ ] T-013 — Review, test and report phase 1
            Depends on: every delivery milestone in Phase 1 being complete and closed; tasks T-004, T-008, T-012.
            Evidence: review cross-milestone code, verify phase exits and prior behavior, fix blockers and commit docs/dev/reports/phases/1/PHASE-REPORT.md with findings/TODOs; reconcile review milestone, integrate explicitly, verify merged state and publish. Prepare v0.1.0 release notes/tag/release only after verified integration and within authorized execution.
            Include docs/dev/reports/IMPLEMENTATION-REPORT.md with full task-list result and aggregated unresolved/deferred TODOs, options and provenance.
