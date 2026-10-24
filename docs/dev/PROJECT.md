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

## Platform and constraints

Implement the game in TypeScript using a DOM/CSS board and native browser controls. Deliver it as a static browser application without a backend, account or external service requirement. Keep gameplay logic independently testable from rendering and input handling.

Prioritize Chromium and include Firefox verification under the repository's [browser development policy](../../BROWSER_DEV_POLICY.md). The browser interface must make the game state readable and restart accessible through the keyboard.

## Boundaries

This is a local single-player game. Multiplayer, accounts, online leaderboards, audio, touch-specific controls and saved gameplay sessions are outside its scope.
