# Project Memory

Last updated: 2026-11-22

This file records the game's major requests, implementation decisions, and validation notes so future work can continue without losing context.

## Project shape

- The game is implemented primarily in `index.html`, with inline Three.js scene/game logic and UI.
- Progress is saved in browser local storage under `honeybee_sim_save_v1`.
- `README.md` is the short public overview; keep this file focused on development history and decisions.

## History

1. **Interaction and opening sequence**
   - Dialogue is tap/click controlled rather than advancing automatically.
   - Contextual action labels distinguish gains (`+`) from costs.
   - The repaired workbench triggers a wife-led tutorial conversation about the ladder and reaching the hive. The wife walks over to the husband, then returns toward the home entrance.
   - The opening follows the bee from outside the fence, past the hive, and toward the couple with a linear camera move. The bee greets the player before the couple's conversation.
   - Prologue music is `assets/Music/Prologue_bee_entry.mp3`; its intro and dialogue/background levels were tuned in earlier work.

2. **Roadside villager market**
   - Added the visitor gate/path, striped-awning stall, walk-up customers, a queue of up to two, order bubbles, tips, reputation, and order history.
   - The first customer in a fresh game is Susmitha.
   - Quick-sale and flower-planting remain available when there is no customer to serve.

3. **Honey storage and shop progression**
   - The cupboard stores harvested honey. The first deposit unlocks the debris-filled construction plot by the front-left fence.
   - Building the shop is a separate action; the finished stall faces sideways.
   - Customer orders and quick sales count both carried and stored honey. Taking honey back out does not relock an already unlocked shop.

4. **Returning a picked-up hive**
   - A populated hive picked up from the tree can now be returned to an open tree spot. Every free tree spot is marked while carrying a hive, and the action targets the nearest free spot.
   - Returning preserves the hive data, including its queen/population, tier, timer, and ant state. Placing it on a hive stand remains available.
   - The rack still stores regular bee boxes; temporary storage of populated hives on the rack was not added because free tree spots provide a return path.

5. **Surface texture pass**
   - Added subtle, repeating procedural textures for grass, dirt paths, cottage plaster, and the wood palette while retaining the existing toon-shaded style.
   - Textures are generated locally with small canvas tiles; no external assets or dependencies were added.

6. **Dialogue readability**
   - Enlarged the dialogue text and panel, with a slightly smaller but still larger-than-before text size on screens under 400px.
   - Kept the panel inside the mobile safe-area and widened it modestly on narrow screens.

7. **Initial resource mission**
   - Before the workbench is repaired, the goal pill shows collected wood and scrap against the required 6 and 4.
   - Once both totals are met, the husband pauses movement for a one-line, tap-to-close message. Closing it marks the tutorial beat complete and changes the goal to repairing the workbench.

8. **Sequential hive-building tutorial**
   - After the wife’s workbench conversation, the guide advances in order: craft the A-frame ladder, pick it up and place it at the wild tree, craft and place an empty Bee Box, climb and smoke the bees, capture and place the Queen, craft/place a Hive Stand, then pick up and mount the populated hive.
   - The climb prompt and climb action stay locked until an empty Bee Box has been placed at a tree marker.
   - During the ladder-pickup step, its nearby prompt takes priority over the workbench prompt so the player can pick it up without circling the bench.
   - At each step, the guide exposes only the expected action. During craft steps, only the required recipe is enabled. Missing materials are explained, and players can close the workbench to gather more.
   - The guide is derived from existing progression fields and clears after the first populated hive is mounted on a stand; no save migration is needed.
   - Full-scene dimming is now only a subtle 450 ms flash when a tutorial step changes. After the flash, the goal and focused action remain clear without dimming the world or settings controls.

9. **Seasonal honey and Queen cooldowns**
   - The repeating cycle is nine real-time hours: Bloom (2h at 1.5×), Normal (4h at 1×), Dearth (2h at 0.5×), and Dormant (1h at 0×). That is eight hours of base production work per full cycle. The cycle anchor is saved per game rather than tied to the calendar; a new game or a legacy save without an anchor starts at Bloom.
   - Seasonal flow multiplies the existing flower bonus (`1 + 0.12 × flowerLevel`) and, for managed hives, the existing tier bonus (T1 1×, T2 1.2×, T3 1.45×). Dormant pauses honey production, but Queen/settling cooldowns still elapse in real wall-clock time.
   - Wild-tree stock starts at 3 jars and is capped at 3. Each jar needs 1,800 productive seconds; managed Bee Box hives need 900 productive seconds per batch. The first managed-hive batch remains 45 productive seconds as the tutorial exception; subsequent batches use 900 seconds.
   - Capturing the tree Queen starts a 60-minute recovery cooldown on the wild source. While recovering, the source cannot be climbed to, smoked, harvested, or used to capture another Queen, and its honey refill pauses. The timestamp is set when capture completes, not when the Queen is later placed.
   - Placing the captured Queen in a Bee Box starts a separate 30-minute settling cooldown. Managed production begins only after the populated hive is mounted on a stand; settling can elapse before mounting, but honey production cannot. A single recovered tree colony can supply later boxes when the player has an empty box available.
   - Offline progression integrates productive time across season boundaries and respects the tree recovery, box activation, and stand-mount timestamps. Hidden-tab time is also applied when the page becomes visible; periodic saves are skipped while hidden so they do not erase that elapsed interval.
   - Added backward-compatible fields to the existing `honeybee_sim_save_v1` save object without replacing old state or changing its version: `seasonStartedAt`, `treeQueenReadyAt`, `wildHoney` (0–3), and `wildHoneyProgress` (productive seconds toward the next tree jar). Hive records carry `activationReadyAt` and `productionStartedAt`. Existing saves receive defaults; no explicit migration is needed.
   - Floating labels show the current nectar phase and wild stock, tree recovery time, box settling time, and real-time estimated harvest countdown. Save/load failures are logged; save failures also update the save-status label.

## Validation notes

- Browser-tested the storage unlock/build flow, Susmitha's order using stored honey, and narrow-screen HUD fit.
- Browser-tested picking up and returning a populated hive, including persistence of the returned hive state.
- Browser-checked the new surface textures at desktop and 360px mobile widths; no runtime errors or horizontal overflow.
- Previewed a long dialogue line at desktop and 360px widths; the enlarged panel remained within the viewport.
- Checked partial and complete resource mission states, the husband's message, movement pause, and the post-dialogue workbench objective.
- Browser-tested the ladder and Bee Box craft gates, the Hive Stand recipe gate, and the focused Bee Box placement action.
- Checked the updated game page in the browser and `git diff --check`; the browser’s local-file audio request remains an unrelated aborted request.
- `git diff --check` passed after the recent changes.
- Browser-tested a two-hour offline fixture spanning Bloom into Normal: 20 productive Bloom minutes yielded one tree jar, and a stand hive with an elapsed activation cooldown became ready.
- Browser-tested future offline cooldowns: the tree displayed about 60 minutes of Queen recovery and the hive displayed about 30 minutes of settling without producing early.
- Browser-tested a 45-second first batch after 20 seconds offline in Bloom: 30 productive seconds were applied and the remaining countdown stayed near 10 seconds.
- Reloaded and ran the game in the browser with no JavaScript runtime errors. The local-file prologue audio request can show an unrelated aborted-request event.

## Continuation guidance

- Preserve existing save fields when adding state; add defaults and normalize older saves rather than replacing save data.
- For interaction changes, update both the contextual prompt-selection logic and its action handler.
- After gameplay changes, test the actual in-game sequence in a browser and check `git diff --check`.
