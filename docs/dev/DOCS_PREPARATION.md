# Main documentation preparation record

## Current control

- Mode: autodoc/autoloop, explicitly requested by the user on 24 October 2026.
- Current scope: pass 6, cumulative scope through item 6 of the ordered scope sequence.
- Approval basis: the first PROJECT approval was received on 24 October 2026 for proposal commit `30e2e9c0c195874d8eae2a184eb75d5470927806` (PROJECT blob `7a04386debc8ac2b4d9f14ac563f70139c4606c9`). Later PROJECT approvals are waived; ARCHITECTURE through TASKS checkpoints are preapproved in every pass. This policy was announced before the first brief. The user's “Yes” confirmed the proposed gameplay interactions and TypeScript/DOM direction; it did not approve an unseen PROJECT.
- Decision delegation: after the first PROJECT approval, resolve new material decisions within the predefined scopes using agent judgment, with visible rationale and durable owning-document decisions.
- Tracking: enabled for documentation and implementation by root AGENTS.md. Native milestone 1, `sdd-docs-main`, and issues 1–9 correspond to DOCS-001–009; exact standard content and associations were read back before repository modifications.
- Publication: push and verify each owned commit before advancing. Reporting: Standard after Explicit entry reporting.
- Next boundary: publish/close DOCS-008, then stop before DOCS-009, package QC, integration and implementation.
- Final authorized stop: after pass 6 DOCS-008 publication and issue closure, before DOCS-009, whole-package QC, documentation integration and implementation. Keep DOCS-009 and the documentation milestone open.

## Scope sequence

Each pass revises the retained complete document set through DOCS-002–008. Reset the same task checkboxes and reopen the same issues between predefined passes; DOCS-001 remains complete. Product documents describe the resulting current scope without preparation-process narrative.

1. **Classic baseline:** falling seven tetrominoes on a 10 × 20 board; keyboard movement, rotation, gravity, line clearing, scoring, game over and restart. Keep the scope minimal and coherent.
2. **Gameplay enhancements:** hold, ghost, 7-bag randomization, wall kicks, hard drop and a default full-gravity-interval lock delay, including after hard drop.
3. **Conditional ghost:** dynamically hide the ghost above the board midline and show it again in the lower half, using the agreed straddle rule below.
4. **Penetrator:** activate the current falling piece with a hotkey only when a bonus is available. It passes through occupied cells without destroying them and must lock entirely within the board without overlap. Hard drop passes obstructions to the lowest valid landing; the visible ghost previews exactly that landing. Each four-line clear earns one activation, with a capacity of three bonuses. Activated pieces are entirely white; regular pieces never use white. Show one side-panel symbol per available bonus and update on earning or spending.
5. **Session configuration:** side-panel controls for ghost visibility (hide/dynamic/show), hard drop (off/on), next preview (off/on), hard-drop locking (follow general/immediate), general locking (failed downward attempt/full gravity interval) and hold (off/on). Add next preview. Settings survive game restart within the page but do not persist across reloads in this scope.
6. **Configuration persistence:** support a URL parameter and cookie for each setting, resolving independently as valid explicit URL value → valid cookie → default. Save every valid explicit URL value in its cookie, even when equal to the default. An omitted parameter retains its cookie. User changes write the corresponding cookie and update the corresponding query parameter, preserving unrelated parameters. Restore on reload and reopening.

## Agreed cross-scope decisions

The user confirmed the following proposals with “Yes” on 24 October 2026. These are retained here until their scopes become current; adopt the corresponding contracts in their owning product documents at that point.

- Technology: TypeScript with a DOM/CSS board and native configuration controls, delivered as a static browser application. No backend is required.
- Ghost midline: hide a dynamic ghost if any projected cell is above the midline; show it only when every projected cell is in the lower half.
- Delayed locking: start one full gravity interval when grounded. Grounded movement/rotation does not restart the timer. Becoming airborne cancels it; grounding again starts a fresh interval. Hard drop starts a full delay unless immediate hard-drop locking is selected.
- Penetrator hold/activation: disable hold for an activated penetrator. Successful activation spends one bonus; repeated activation on that piece is unavailable. The ability ends when it locks. Restart clears bonuses.
- Penetrator landing: search through obstructions for the lowest completely in-board, non-overlapping position. Ghost and hard drop share that result. Reject activation or movement that would leave no valid landing at or below the piece. Preserve occupied board cells throughout traversal.
- Live configuration: visibility and control toggles apply immediately. Locking-policy changes apply to the next piece and preserve the current piece's timer/policy. Disabling hold retains the stored piece.
- Invalid values: an invalid URL value falls back to the valid cookie for that setting, then its default. Invalid cookies fall back to defaults. Explicit valid URL values, including default values, are saved to cookies.
- Final defaults: dynamic ghost, hard drop on, next preview on, hold on, full-gravity-interval general locking and hard drop following general locking.
- Versioning: first playable release v0.1.0; increase versions to reflect delivered phase scope. The user authorized a GitHub release with matching tag/notes after each subsequently authorized implementation phase is completed, verified and integrated. No implementation range is currently authorized and no release is due during preparation.

## Decisions and document evidence

| Task | Current evidence / decision basis |
| --- | --- |
| DOCS-001 | All four root records existed at baseline `19e560c7b3fb680c2589902ba8c3466f795249d4`; retained byte for byte. DOCS_TASKS is instantiated from the running package with only DOCS-001 checked. This record and HANDOFF are established in the same initialization commit. |
| DOCS-002 | Scope 6: `PROJECT.md` blob `27b571b16e6c78316d7feea10a83b677439c4afa`; accepted under autodoc/autoloop checkpoint preapproval, owner sdd-design. Owner consistency/content review passed. |
| DOCS-003 | Scope 6: `ARCHITECTURE.md` blob `20928494cd7c2006544dcf0631de1d2741ab2643`; accepted under autodoc/autoloop checkpoint preapproval, owner sdd-design. Owner consistency/content review passed. |
| DOCS-004 | Scope 6: `DECOMPOSITION.md` blob `338cd050cb7d66481e2b6489e1dde3e54aa515de`; accepted under autodoc/autoloop checkpoint preapproval, owner sdd-design. Owner consistency/content review passed. |
| DOCS-005 | Scope 6: `SPEC.md` blob `79517958465616837a7e78cf46c2c09050186863`; accepted under autodoc/autoloop checkpoint preapproval, owner sdd-specify. QC: `SPEC-REVIEW-REPORT.md` (blob `8d754358927282dc26a51d4da7a9e8f559660944`). |
| DOCS-006 | Scope 6: `PLAN.md` blob `1b63d0524b3ebda8b9e7d76c313c74cd7cfd35d0`; accepted under autodoc/autoloop checkpoint preapproval, owner sdd-plan. QC: `PLAN-REVIEW-REPORT.md` (blob `0dd2ce3f973372363182e77f2c77636594cb2e2c`). |
| DOCS-007 | Scope 6: `layout.md` blob `b4d204361c6f8817060dc3657bb6371ba7150959`; accepted under autodoc/autoloop checkpoint preapproval, owner sdd-plan. QC: `PLAN-REVIEW-REPORT.md` (blob `526b5fbd6188fdc44aa82c23158bee81bb6f6692`). |
| DOCS-008 | Scope 6: `TASKS.md` blob `29fe918cbe4de001de0fb792bbc81e202cfe15cb`; accepted under autodoc/autoloop checkpoint preapproval, owner sdd-tasks. QC: `TASKS-REVIEW-REPORT.md` (blob `36e984b647c98e7f6c4fef03e4f86b1b800a5703`). |
| DOCS-009 | Unchecked and unselected; remains outside the authorized autoloop boundary. |

Material unresolved decisions: none blocking the initial brief. Exact gameplay contracts belong to SPEC; architecture, delivery and task details remain with their respective owners.

## Initialization and current reset

- Retained existing root files: `AI_DISCLOSURE.md`, `SDD-MANAGER.md`, `AGENTS.md`, `SKILL_DISCOVERY_PROTOCOL.md`.
- Copied root files: none. All four destinations are ordinary files. Byte comparison against the initial committed tree passed for every retained file.
- Initialization continuation: all four initially existed; no mixed-presence continuation pause applies.
- Package: repository-vendored SDD Manager 0.15.2; discovery completed. Linux host, Git, Node 24.19.0 and Python 3.12.14 are available. Root Windows shell policy is inapplicable to this execution host. Browser-independent documentation needs no browser provisioning; BROWSER_DEV_POLICY governs subsequent browser design/setup/verification.
- Current reset: prior scope 5 completed through DOCS-008 at `0b51208a461fd92f13fd0c5129720a2fbb8e291f`; advance to predefined cumulative scope 6. Authorized by first PROJECT approval and requested autoloop; retain all document bytes as revision inputs and reset only checkboxes/control.
- Prior-pass evidence: none.

## Pending transitions

The current scope-6 DOCS-008 checkpoint awaits publication and exact issue-8 closure readback at the time of this record. Earlier required checkpoint effects were verified before this document was authored; Git and issue evidence identify those commits. Reconcile actual later effects before replay. After closure, stop before DOCS-009.
