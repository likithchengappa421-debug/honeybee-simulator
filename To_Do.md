# Game Review To-Do

Review date: 2026-11-22

Items are sorted by impact on gameplay, progression, and player retention.

## Priority 0 — Fix first-session blockers

- [x] **Shorten time to first honey.** The first populated Bee Box now skips the 30-minute settling cooldown so its 45-second tutorial harvest can start after mounting. Later colonies retain the 30-minute cooldown.
- [x] **Reduce the early resource grind.** Repairing the workbench now gives a one-time 4 wood and 2 scrap toward the ladder, while preserving recipe costs and the need to gather more materials.
- [x] **Make the first market order fulfillable.** Susmita's guaranteed first order is now 1 jar, so the first tier-1 harvest can fulfill it after the jar unlocks the shop.
- [x] **Prevent an understocked customer from blocking the market.** Understocked orders can be skipped, quick-sell remains available at the counter, and a skipped customer leaves so the queue can advance.

## Priority 1 — Improve progression and return motivation

- [ ] **Add post-tutorial objectives.** Replace the vague “Expand your apiary” goal with a short optional chain: harvest honey, store it, build the stall, serve the first customer, plant flowers or upgrade a hive, and prepare another box.
- [x] **Make the one-hour Queen recovery period productive.** The wild-tree recovery is waived until the first managed-hive harvest, including active onboarding saves. Later captures retain the full hour, with the goal pointing to useful apiary or market activities during recovery.
- [x] **Explain the controls and collection rules.** The first-play screen now explains WASD/arrow keys or the on-screen joystick, tapping/clicking contextual prompts, and automatic wood/scrap collection by proximity. The hint is hidden on later plays.
- [x] **Keep guidance, but allow some exploration.** The current tutorial goal and required build/capture actions remain highlighted and gated, while players can harvest, use storage and the market, tend/upgrade existing hives, climb down, and craft an optional Box Rack.

## Priority 2 — UI and platform polish

- [x] **Prevent floating labels from covering the HUD.** The goal is positioned below the actual resource HUD height, and hive tags are clipped below the goal; the wild-hive label is clamped into that area.
- [x] **Review desktop layout.** The desktop portrait frame and arrow-key movement were checked at 1440×900. Retain the existing layout for now; a landscape option needs separate design and playtesting rather than changing the default.
- [ ] **Review accessibility and mobile performance.** Pinch zoom is enabled and checked in mobile emulation. Frame-rate profiling on real phones is still needed before adding more scenery; the scene uses many individual shadow-casting meshes.

## Preserve these strengths

- Keep the cohesive cozy, low-poly apiary art direction and the wife-led opening/tutorial story.
- Keep tap-controlled dialogue, contextual action prompts, and focused progression guidance.
- Keep proximity-based resource collection, the customer tip/reputation loop, quick-sale fallback, and flower/hive upgrades.
- Keep offline progression and backward-compatible save data; cooldowns should not require leaving the game open.

## Playtest checks

- [ ] From a fresh save, measure time to first useful honey, shop unlock, and first successful customer order.
- [x] Confirm the first customer can be served with the stock available at shop unlock. Browser fixtures carried one tier-1 harvest through storage, shop construction, and Susmita's order.
- [ ] Check that no long stretch leaves the player without a useful action.
- [x] Test HUD/tag overlap and controls on a narrow phone screen and a desktop viewport. Checked 320×568 and 1440×900, including a wrapped resource HUD, keyboard movement, and an emulated touch joystick; real-device testing remains open above.
