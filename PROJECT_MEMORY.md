# Project Memory

Last updated: 2026-11-24

This file records the game's major requests, implementation decisions, and validation notes so future work can continue without losing context.

## Project shape

- The game is implemented primarily in `index.html`, with inline Three.js scene/game logic and UI.
- `assets/` contains standalone reference snapshots of procedural models, textures, sounds and UI artwork; the game still generates these at runtime.
- Progress is saved in browser local storage under `honeybee_sim_save_v1`.
- `README.md` is the short public overview; keep this file focused on development history and decisions.

## History

1. **Interaction and opening sequence**
   - Dialogue is tap/click controlled rather than advancing automatically.
   - Contextual action labels distinguish gains (`+`) from costs.
   - The repaired workbench triggers a wife-led tutorial conversation about the ladder and reaching the hive. The wife walks over to the husband, then returns toward the home entrance.
   - Completing the workbench repair gives a one-time 4 wood and 2 scrap reward toward the ladder. Recipe prices remain unchanged; the wife explains the saved materials during the tutorial conversation.
   - The opening follows the bee from outside the fence, past the hive, and toward the couple with a linear camera move. The bee greets the player before the couple's conversation.
   - Prologue music is `assets/Music/Prologue_bee_entry.mp3`; its intro and dialogue/background levels were tuned in earlier work.

2. **Roadside villager market**
   - Added the visitor gate/path, striped-awning stall, walk-up customers, tips, reputation, and order history. The stall sits in the left entrance corner, parallel to the fence.
   - The first customer in a fresh game is Susmita.
   - Three introductory customers, starting with Susmita, each request one jar. Customers wait at the gate until the player enters, then approach the counter one at a time; they no longer time out and later completed orders earn a tip. Introductory sales persist; existing shops can meet them too. Later orders retain their existing random 1–3 jar quantities.
   - Entering the shop opens a face-to-face camera and tap-controlled customer dialogue. Later understocked orders can be skipped, and quick-sell remains available inside the shop. Flower planting remains available outside.

3. **Honey storage and shop progression**
   - The cupboard stores harvested honey. The first deposit unlocks the debris-filled construction plot by the front-left fence.
   - After the first deposit, the couple discusses the honey journey and plans a shop at the entrance. Building it costs 10 wood and 10 scrap. Existing built shops stay built without another charge.
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
   - Tree-Queen recovery is waived through the first managed-hive harvest to keep onboarding moving. A legacy onboarding save with an active recovery timer is cleared on load. Later captures start the full 60-minute cooldown; during it, the wild source cannot be climbed to, smoked, harvested, or used to capture another Queen, and its honey refill pauses. The timestamp is set when capture completes, not when the Queen is later placed.
   - During later recovery, the goal recommends a ready-hive harvest, storing honey to unlock the shop, building the roadside shop, serving or quick-selling honey, or gathering supplies/improving the apiary based on current state.
   - Placing the captured Queen in the first colony skips the 30-minute settling cooldown, allowing its first 45-second tutorial harvest to begin once mounted on a stand. Later colonies retain the 30-minute settling cooldown. Managed production begins only after the populated hive is mounted on a stand; settling can elapse before mounting, but honey production cannot. A single recovered tree colony can supply later boxes when the player has an empty box available.
   - Offline progression integrates productive time across season boundaries and respects the tree recovery, box activation, and stand-mount timestamps. Hidden-tab time is also applied when the page becomes visible; periodic saves are skipped while hidden so they do not erase that elapsed interval.
   - Added backward-compatible fields to the existing `honeybee_sim_save_v1` save object without replacing old state or changing its version: `seasonStartedAt`, `treeQueenReadyAt`, `wildHoney` (0–3), and `wildHoneyProgress` (productive seconds toward the next tree jar). Hive records carry `activationReadyAt` and `productionStartedAt`. Existing saves receive defaults; no explicit migration is needed.
   - Floating labels show the current nectar phase and wild stock, tree recovery time, box settling time, and real-time estimated harvest countdown. Save/load failures are logged; save failures also update the save-status label.
10. **First-play controls hint**
   - Added a compact, non-blocking hint to the Play screen: move with WASD/arrows or the on-screen joystick, tap/click contextual action prompts, and walk near wood or scrap to collect them automatically.
   - Added backward-compatible `controlsHintSeen` save state. It defaults false for fresh and existing saves, then becomes true and is saved when Play is pressed so the hint is hidden on subsequent visits.
   - Kept the hint responsive on narrow mobile screens; it appears before gameplay rather than interrupting the opening/tutorial.
11. **Tutorial guidance with optional exploration**
   - The active progression goal and required tutorial actions remain highlighted/gated in their existing sequence.
   - While a tutorial step is active, optional market and storage interactions, honey harvesting, hive tending/upgrades, and climbing down remain available. The Box Rack recipe is also available in the Workbench; other off-sequence recipes stay gated.
   - Optional prompts remain visible without receiving the tutorial focus, and the Workbench explains that the highlighted goal stays active while the Rack is optional.
12. **HUD and platform polish**
   - Position the goal below the actual resource HUD, including wrapped rows. Hive status tags use a clipped layer below the goal; the wild-hive tag is clamped into this area.
   - Allow browser pinch zoom on the game surface and ignore additional joystick pointers while a joystick gesture is active. The portrait desktop frame remains unchanged after review; a landscape option would need separate design and playtesting.
   - Real-phone frame-rate profiling remains outstanding; browser emulation is not a substitute for real devices. See the later validation notes for the fresh-save browser run.
13. **Transactional crafting**
   - Materials are checked when crafting starts but deducted only when the finished item is created and saved. Interrupting the animation leaves the saved materials intact, so the player can retry; resetting the save cancels the animation.
14. **Climbing down during the hive tutorial**
   - Climb Down remains available as an optional action while the smoke or Queen capture step is highlighted. It becomes the highlighted action when carrying the Queen, and the guide resumes at climbing or Queen placement after descending.
15. **Customer wait limit**
   - The previous 60-second departure and 18-second tip timers were removed when customers began waiting at the gate until the shop opens. Manual skip and quick-sell remain available for later customers.
16. **Post-tutorial objectives**
   - After the first hive is mounted, optional goals guide the first harvest, storage, shop construction, first customer sale or quick sale, and preparing another colony. Existing carrying, placement and hive-mounting guidance takes precedence when needed; activities remain unlocked.
   - `firstMarketSaleDone` records customer sales and quick sales. Older saves with served orders inherit the milestone; an old quick sale cannot be inferred reliably and may prompt one more sale. Other objectives derive from existing hive, box, storage and shop state.
   - Goals refresh when a hive becomes ready or the wild-tree recovery state changes, so the first harvest and next-colony guidance can advance without another player action.
17. **Keyboard controls**
   - Contextual prompts and Climb Down are native buttons; dialogue accepts Enter or Space. Focus enters dialogue and menus, remains within them while open, and returns to a game control on close. Escape closes the workbench and settings.
   - Movement pauses in menus and held keys are cleared on opening/closing them or losing window focus. The start screen keeps the underlying toolbar out of the tab order.
18. **Production timing and milestones**
   - Live honey production uses the same elapsed-time seasonal integration as offline progress. It advances before saving and before production-affecting actions; a new harvest batch starts at its harvest timestamp. Animation and movement retain their capped delta.
   - Optional milestones award 4 wood for the first managed harvest, 4 scrap for the first customer order, and 5 coins for two mounted populated hives. The settings menu shows progress, and `milestonesClaimed` persists the grants. Saves predating the field mark already-completed milestones without granting retroactive resources.
   - Removed the unsupported breeding claim from the README; no breeding mechanic was added. The existing portrait layout and audio remain by choice, with landscape and ambience deferred.
19. **Balanced visual pass**
   - Rebalanced the meadow, foliage, flowers, terracotta roof, honey accents, lighting and outlines while retaining the low-poly/toon style and recognizable interaction highlights. After the first pass appeared too muted, restored some saturation and warmth.
   - Added lightweight character details, booted legs, an apron, cottage window panes and bee antennae. Existing movement, collision and interaction areas remain unchanged; small decorative details do not cast shadows.
20. **First smoker tutorial**
   - The opening and bench conversation are shorter. A larger goal bar, green action highlights and ground trails guide repair, crafting, box placement and the smoker materials; the trail ends near the destination.
   - The first unsmoked wild-honey attempt startles the bees and automatically brings the player down without taking honey. The husband then collects coconut husk and a tin can and crafts a crude smoker at the workbench before returning to smoke the hive.
   - After descending, player-advanced thoughts show his relief and gentle-smoke idea before the camera reveals the coconut tree; the tin can and workbench are revealed in later beats after each pickup.
   - The coconut palm has a taller curved trunk, fronds and coconuts in its crown; its tutorial camera frames both the crown and the fallen husk.
   - Workbench guidance clears stale disabled styling when switching tutorial recipes, so the smoker remains craftable after earlier guided steps.
   - `smokerStage` persists the new tutorial. Saves predating it retain smoker access; an interrupted startled retreat resumes at the coconut step. The carried ladder is upright and kept clear of the oak, and its landing position avoids the trunk.
21. **Settings controls**
   - Sound and settings controls are larger and positioned on the left. Quit saves before returning to the title; if saving fails, the settings menu stays open and reports the error.
22. **First hive storage guidance**
   - After the first hive is mounted, the husband explains that harvested honey goes in the cupboard. The first hive harvest points back to storage even if the roadside shop was already unlocked with wild honey; the guidance remains optional.
   - First-hive storage and introduction progress are saved separately. Older saves with a completed hive harvest retain their existing progression.
23. **Fresh-run starter cache**
   - Fresh games begin with 2 wood and 2 scrap, and the opening dialogue explains that the husband saved a little material. Existing saves retain their saved resource totals; recipe costs and tutorial order are unchanged.
24. **Repeatable community honey contracts**
   - After the three introductory shop customers, sales to regular customers count toward a repeatable contract: deliver five jars for 10 coins. Contract progress and completed count are saved; quick-selling does not count. Existing flower planting and hive tier upgrades remain the intended coin sinks.
25. **Nectar timing clarity**
   - The in-game season status shows the active nectar multiplier and time until the next phase. Hive tags show their effective multiplier, including flower and tier bonuses, alongside the estimated real-time harvest wait. Production values and cooldowns are unchanged.
   - Browser-checked the Bloom rate and next-phase countdown at 320×568; the status fits between the goal and hive-tag area. The status stays hidden on the title screen and during the opening cutscene.
26. **Smoker tutorial risk clarity**
   - Before the scripted first unsmoked harvest, the goal, arrival banner, and action prompt warn that bees may swarm. The startled-bees story and existing progression remain unchanged.
   - Browser-loaded an isolated smoker-tutorial fixture through the Climb Ladder prompt. The browser tab remained background-hidden, so the climb and warning prompt could not be interactively verified.
   - `git diff --check` passed.
27. **Three.js resource cleanup**
   - Removed customer and hive actors, stand models, flying pickups, and transient particles now dispose their owned geometries and materials. Shared bee/effect geometries and cached toon/outline materials are excluded from disposal.
   - Browser-loaded the game with no JavaScript page errors. A long market-session check was not possible because the browser test tab is background-hidden; `git diff --check` passed.
28. **Customer live-region updates**
   - The customer status now updates only when the text changes, avoiding repeated mutations and announcements from the animation loop.
29. **Reset failure handling**
   - If local storage rejects save removal, reset now logs the error, keeps the game and settings modal intact, resets the confirmation button, and reports that progress is unchanged.
   - Browser-tested a simulated `SecurityError`: the modal remained open and the 26 wood/18 scrap save was preserved. Restoring storage access still allowed reset to complete.
30. **External runtime fallback**
   - Online CDN delivery remains intentional. If Three.js is unavailable, a screen-reader alert explains the connection/reload step; the optional Fredoka font keeps its system-font fallback. The README documents the internet requirement.
   - Browser-blocked the Three.js CDN and confirmed the accessible alert replaces the unusable game UI; after unblocking, Three.js and the renderer loaded with no JavaScript page errors.
   - `git diff --check` passed.
31. **Cleaner title screen**
   - Increased the title backdrop gradient opacity to 96% so HUD and objective details no longer compete with the title, while preserving a faint view of the game scene.
   - Browser-checked the 320×568 layout and confirmed Play still transitions into the opening cutscene.
   - `git diff --check` passed.

## Validation notes

- Browser-tested the storage unlock/build flow, Susmita's order using stored honey, and narrow-screen HUD fit.
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
- Browser-tested isolated fixtures: the first populated colony has no settling countdown and reports its 45-second first harvest; a later colony still shows a 30-minute settling cooldown.
- Browser-tested a legacy onboarding save with a future Queen recovery timer: loading cleared the timer. A later-recovery fixture preserved nearly 60 minutes and suggested harvesting a ready managed hive.
- Browser-tested an isolated workbench repair flow: the repair still costs 6 wood and 4 scrap, then grants 4 wood and 2 scrap, and the tutorial dialogue explains the saved ladder materials.
- Browser-tested the first-play hint at 320×568: all hint text and the Play button fit inside the game frame. Pressing Play persisted `controlsHintSeen`; a returning-save fixture hid the hint.
- Browser-tested a fixture at the ladder-crafting tutorial step: the ladder remained focused, off-sequence Bee Box and Stand recipes were disabled, and the optional Box Rack recipe remained available.
- Checked the HUD, goal and wild-hive label at 320×568 and 1440×900 with resource totals large enough to wrap the HUD. Keyboard movement worked at both sizes, and the joystick moved the player with emulated touch input. Chromium mobile emulation zoomed from 1× to 1.6× using a pinch gesture.
- Carried state between isolated browser fixtures for a ready tier-1 harvest, storage, shop construction, and Susmita's first order: the single harvested jar unlocked the shop and fulfilled the order. The fixtures repositioned the player and do not measure fresh-save completion time.
- Browser-tested manual saving and reloading during each of the five crafting animations: materials remained available. Retrying each craft produced its item and charged its normal cost, including the workbench repair reward. Resetting during a ladder craft left the fresh save unchanged.
- Browser-tested climbing down during both smoke and capture steps at 390×844, returning to the hive, and descending with the Queen; the appropriate tutorial action remained highlighted throughout. Also checked smoke-step descent at 1440×900.
- Browser-tested full 60-second waits with and without stock: Susmita left without a sale or reputation change, preserving available stock, and the next customer reached the counter with a fresh countdown. Also checked serving, quick-selling then skipping, and pausing the countdown in settings at 320×568; simulated visibility changes also paused the countdown.
- Browser-tested the first-hive readiness transition and the harvest → storage → shop → Susmita sale objective chain using isolated fixtures. Checked quick-sale progression, legacy served-order saves, second-box crafting and reload, a box in the rack, and automatic guidance after wild-tree recovery.
- A fresh-save desktop browser run, skipping the opening and including automation pauses, reached the first managed honey at roughly 5:03, shop unlock at 5:11, and a successful one-jar Susmita order at 5:36. The player gathered materials while the first hive filled. These are not human speed benchmarks; later recovery and mid-game pacing still need extended playtesting.
- Checked keyboard focus, dialogue advancement, workbench crafting and modal Tab/Escape, plus simulated touch on the workbench prompt at 320px. Checked milestone grants, persisted claimed IDs, legacy-save migration and a second-hive mount without duplicate grants.
- Checked seasonal/activation boundaries and repeated saves, as well as a simulated five-frame-per-second production run: approximately 5.1 productive seconds accumulated in 3.3 real seconds during Bloom. Real-phone frame-rate profiling remains outstanding.
- Visually checked the revised scene and opening dialogue on desktop and a 360px emulated phone. The game loaded without JavaScript errors, the mobile layout had no horizontal overflow, and keyboard movement still collected wood. Real-phone performance was not measured.
- Browser-tested isolated tutorial states through box placement, unsmoked harvest, automatic retreat, coconut and tin collection, smoker crafting, and the subsequent smoke/capture prompt. Checked the repair trail and highlight at 360px, no-box ladder placement and old-save smoker migration. These fixtures repositioned the player and are not an end-to-end playthrough.
- Set Susmita's guaranteed first order to one jar. An isolated interactive customer-spawn test could not be completed because the browser fixture remained in a hidden tab.
- Added counter actions for skipping understocked orders and quick-selling honey; the updated game page loaded, but the hidden browser frame prevented an interactive queue-flow test.
- Reloaded and ran the game in the browser with no JavaScript runtime errors. The local-file prologue audio request can show an unrelated aborted-request event.
- Browser-tested a fresh HTTP-served game: it starts at 2 wood and 2 scrap and shows 2/6 wood, 2/4 scrap toward the repair goal. A returning-save fixture retained its saved 26 wood and 18 scrap; the test fixture was removed afterward.
- Browser-tested a regular customer sale at 4/5 contract progress: it completed the contract, awarded 10 coins, incremented the saved completion count, and reset progress to zero. The test fixture was removed afterward.

## Continuation guidance

- Preserve existing save fields when adding state; add defaults and normalize older saves rather than replacing save data.
- For interaction changes, update both the contextual prompt-selection logic and its action handler.
- After gameplay changes, test the actual in-game sequence in a browser and check `git diff --check`.
