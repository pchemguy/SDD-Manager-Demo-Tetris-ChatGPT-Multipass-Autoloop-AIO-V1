# Browser Tetris

## Purpose and users

Provide a playable single-player Tetris game in a web browser for people who want a straightforward keyboard-controlled game. A player can start immediately, play until game over and restart without reloading the page.

## Intended capabilities

- A 10 × 20 playfield with the seven tetromino shapes, one falling piece at a time.
- Keyboard movement, rotation and soft drop, with automatic gravity.
- Collision handling that keeps pieces inside the playfield and prevents overlap with locked cells.
- Piece locking, complete-row clearing and visible scoring.
- Game over when a new piece cannot enter play, with an explicit restart control that begins a clean game.
- A clear board, score, game status and discoverable keyboard instructions.

The game should remain small, coherent and responsive, with predictable gameplay and controls that do not accidentally scroll the page while playing.

## Gameplay assistance

Provide hold, a landing ghost, seven-piece bag randomization, wall kicks and hard drop. The default locking policy gives a piece one full gravity interval after grounding, including a hard drop that reaches the ground. Grounded lateral movement and rotation do not restart that interval.

In dynamic visibility mode, hide the landing ghost when any projected cell lies above the board midline and show it when the whole projection is in the lower half.

## Penetrator ability

Four-line clears award penetrator bonuses, with at most three stored. A hotkey spends one bonus to activate the current piece until it locks. Activated pieces can travel through occupied cells without destroying them and must settle entirely within the board without overlap. Hard drop and the visible ghost share the lowest valid landing position. Hold is unavailable for an activated penetrator.

Render activated pieces entirely white, reserve other colors for regular pieces and show one side-panel symbol for each available bonus. Restart clears bonuses.

## Player configuration

Provide native side-panel controls for ghost visibility, hard-drop availability, next-piece preview, hard-drop locking policy, general locking policy and hold availability. Show the next piece when enabled. Visibility and control toggles take effect immediately; locking-policy changes take effect with the next piece. Disabling hold retains its stored piece.

Default to a dynamic ghost, enabled hard drop, next preview and hold, full-gravity-interval locking and hard drop following the general policy. Settings persist through cookies and can be supplied through URL query parameters. Resolve each setting independently from its valid explicit URL value, otherwise its valid cookie, otherwise its default. Save explicit valid URL values in cookies, including defaults; omitted parameters preserve saved values. User changes save the affected cookie and update its URL parameter. Invalid values fall back independently without preventing play.

## Platform and constraints

Implement the game in TypeScript using a DOM/CSS board and native browser controls. Deliver it as a static browser application without a backend, account or external service requirement. Keep gameplay logic independently testable from rendering and input handling.

Prioritize Chromium and include Firefox verification under the repository's [browser development policy](../../BROWSER_DEV_POLICY.md). The browser interface must make the game state readable and restart accessible through the keyboard.

## Boundaries

This is a local single-player game. Multiplayer, accounts, online leaderboards, audio, touch-specific controls and saved gameplay sessions are outside its scope.
