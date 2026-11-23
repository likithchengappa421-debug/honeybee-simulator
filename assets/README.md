# Asset reference exports

The game still builds its models, textures and sound effects procedurally in
`index.html`. These files are reference snapshots; editing them does not change
the game, and later game changes do not update them automatically.

- `models/`: GLB snapshots of the farm, scenery, characters and objects. The
  farm scene captures a fresh-game state; separate files include later-game
  objects and variants. Models are static and do not contain the game's
  code-driven animations or interactions.
- `textures/`: PNG snapshots of the generated surface, sky and sign textures.
  Some textures and scenery are randomized, so other runs may look different.
- `sfx/`: WAV recordings of the synthesized effects.
- `ui/`: SVG copies of the embedded icons and background patterns.
- `Music/`: the existing prologue soundtrack used by the game.

The GLB exports approximate toon materials with standard materials and omit the
game's back-face outlines. Lighting, shadows, fog and transparency can look
different in other viewers. Dynamic UI, particles, system emoji, the externally
loaded font and Three.js are not standalone assets in this collection.
