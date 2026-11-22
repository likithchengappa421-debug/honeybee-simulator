# Game Review To-Do

Review date: 2026-11-22

Items are sorted by impact on gameplay, progression, and player retention.

## Priority 0 — Fix first-session blockers

- [ ] **Shorten time to first honey.** The first populated Bee Box currently has a 30-minute settling cooldown, while its 45-second tutorial harvest timer only starts after settling and mounting. Waive or sharply shorten settling for the first colony; retain the 30-minute cooldown for later colonies.
- [ ] **Reduce the early resource grind.** Repairing the workbench spends the initial 6 wood and 4 scrap, then the tutorial asks for 8 wood and 4 scrap for a ladder, 10 wood for a box, and 6 scrap for a stand. Consider a small post-repair material reward or lower tutorial costs.
- [ ] **Make the first market order fulfillable.** Susmitha's guaranteed first order is 2 jars, but a tier-1 hive yields 1 jar per harvest and the first jar is needed to unlock the shop. Set her first order to 1 jar or provide enough initial stock.
- [ ] **Prevent an understocked customer from blocking the market.** Customers currently stay queued when the player lacks honey, and quick-sell is unavailable while a customer is at the counter. Let the player dismiss/skip an understocked order, keep quick-sell available, and prevent an unserved customer from blocking the queue indefinitely.

## Priority 1 — Improve progression and return motivation

- [ ] **Add post-tutorial objectives.** Replace the vague “Expand your apiary” goal with a short optional chain: harvest honey, store it, build the stall, serve the first customer, plant flowers or upgrade a hive, and prepare another box.
- [ ] **Make the one-hour Queen recovery period productive.** Keep the cooldown if desired, but waive or shorten it during onboarding and offer useful activities while later colonies recover. Avoid making active play feel like waiting for a timer.
- [ ] **Explain the controls and collection rules.** Show a first-play hint for WASD/arrow keys or the virtual joystick, tapping contextual actions, and automatic collection when walking near wood and scrap.
- [ ] **Keep guidance, but allow some exploration.** Preserve the highlighted tutorial step while allowing harmless optional activities or offering a tutorial-skip/“explore” choice.

## Priority 2 — UI and platform polish

- [ ] **Prevent floating labels from covering the HUD.** Clamp or reposition the wild-hive status label below the resource/goal area, or move it into a fixed hive-status panel.
- [ ] **Review desktop layout.** The game uses a portrait frame on desktop; consider an optional landscape layout if desktop play is a target.
- [ ] **Review accessibility and mobile performance.** Avoid unnecessarily preventing zoom, and profile frame rate on real phones before adding more scenery; the scene uses many individual shadow-casting meshes.

## Preserve these strengths

- Keep the cohesive cozy, low-poly apiary art direction and the wife-led opening/tutorial story.
- Keep tap-controlled dialogue, contextual action prompts, and focused progression guidance.
- Keep proximity-based resource collection, the customer tip/reputation loop, quick-sale fallback, and flower/hive upgrades.
- Keep offline progression and backward-compatible save data; cooldowns should not require leaving the game open.

## Playtest checks

- [ ] From a fresh save, measure time to first useful honey, shop unlock, and first successful customer order.
- [ ] Confirm the first customer can be served with the stock available at shop unlock.
- [ ] Check that no long stretch leaves the player without a useful action.
- [ ] Test HUD/tag overlap and controls on a narrow phone screen and a desktop viewport.
