# Repository layout

These are intended source/test homes, not a claim that application files already exist. [DECOMPOSITION](DECOMPOSITION.md) owns component boundaries and [PLAN](PLAN.md) owns delivery order.

| Owner | Source home | Verification home |
| --- | --- | --- |
| Geometry | src/core/geometry.ts | tests/unit/geometry.test.ts |
| Shape source | src/core/randomizer.ts | tests/unit/randomizer.test.ts |
| Session rules and state | src/core/session.ts | tests/unit/session.test.ts, tests/unit/scoring.test.ts |
| Clock/input control | src/browser/controller.ts | tests/integration/controller.test.ts |
| DOM view and styling | src/ui/view.ts, src/styles.css | tests/e2e/classic.spec.ts |
| Composition/static shell | src/main.ts, index.html | production build and browser entry flow |
| Landing and kicks | src/core/landing.ts; rotation remains in geometry.ts | tests/unit/landing.test.ts, tests/unit/geometry.test.ts |
| Hold/ghost presentation | session.ts and ui/view.ts | tests/unit/hold.test.ts, tests/e2e/assistance.spec.ts |
| Penetrator mode and rewards | landing.ts, session.ts and ui/view.ts | tests/unit/penetrator.test.ts, tests/e2e/penetrator.spec.ts |
| Typed settings and session defaults | src/config/settings.ts | tests/unit/settings.test.ts |
| Native settings panel and preview | src/ui/settings-panel.ts, ui/view.ts | tests/e2e/settings.spec.ts |
| Pure setting resolution | src/config/resolve.ts | tests/unit/resolve.test.ts |
| Browser cookie and URL adapter | src/browser/settings-storage.ts | tests/integration/settings-storage.test.ts, tests/e2e/persistence.spec.ts |

## Repository and generated assets

- Root tooling: package.json/package-lock.json, tsconfig.json, vite.config.ts, vitest.config.ts and playwright.config.ts. The first setup task establishes and validates actual commands; no command is validated by this layout.
- Browser fixtures: tests/e2e/fixtures.ts, with deterministic model fixtures isolated to test builds or module imports. Production entry assets expose no testing endpoint.
- Documentation: docs/dev contains the governing development roots and adjacent preparation review reports; README.md and docs/user/controls.md explain actual setup and player controls.
- Implementation milestone reports: docs/dev/reports/phases/<phase-id>/<milestone-id>.md; phase reports: docs/dev/reports/phases/<phase-id>/PHASE-REPORT.md; final report: docs/dev/reports/IMPLEMENTATION-REPORT.md.
- Release notes: root RELEASE-NOTES.md, prepared from delivered scope at the release boundary. Root project guidance and policy files retain their roles; vendored .agents packages are not application code.
- Generated node_modules, dist, test-results, playwright-report and the project's explicitly isolated browser cache/profile/download directories are ignored and excluded from production source. Their actual locations and versions are recorded during setup under the central browser policy.

Core modules import no browser/UI/configuration adapters. The controller composes the core and view; a view consumes snapshots and typed callbacks. Tests are split by behavior level, not duplicated for every function. Keep a rule in one owner; an extension may add a cohesive module without creating a second state store.
