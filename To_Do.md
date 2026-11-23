# Game Review To-Do

Review date: 2026-11-23

Items are sorted from highest to lowest impact on gameplay, progression, and player retention.

1. [x] **Make crafting transactional.** **Impact: High** — prevents lost materials and progress. **Category: Fix.** Costs and outputs are saved together when crafting finishes; closing or reloading during the animation leaves the materials available to retry. Resetting the game cancels the pending craft. See crafting in [index.html](./index.html).

2. [x] **Allow climbing down during the hive tutorial.** **Impact: High** — prevents players feeling trapped at the hive. **Category: Fix.** Climb Down remains available during the smoke and Queen capture steps, while the current tutorial objective stays highlighted. See tutorial visuals in [index.html](./index.html).

3. [x] **Automatically clear unserved customer orders.** **Impact: High** — prevents an unattended order from blocking the market queue. **Category: Fix.** Customers leave after 60 seconds at the counter, with a visible countdown; the next customer advances. Manual skip and quick-sell remain available. See the market in [index.html](./index.html).

4. [x] **Add post-tutorial objectives.** **Impact: High** — gives players a clear next purpose. **Category: Change.** Optional goals now guide the first harvest, storage, shop, first sale, and preparing a second colony without locking other activities. See goals in [index.html](./index.html).

5. [ ] **Playtest and balance early and mid-game pacing.** **Impact: High** — long waits or repetitive gathering can drive players away. **Category: Change.** A fresh-save desktop browser run (opening skipped, with automation pauses) reached the first managed honey at about 5:03, shop unlock at 5:11, and Susmita's successful one-jar order at 5:36. Materials were gathered while the first hive filled. These are rough observations, not a human speed benchmark; a longer mid-game and later Queen-recovery playtest remains.

6. [x] **Make interactions keyboard accessible.** **Impact: High** — enables desktop players to complete the game without a mouse. **Category: Fix.** Contextual prompts use native buttons, dialogue supports Enter/Space, and modal focus is managed. Touch remains supported. See interactions in [index.html](./index.html).

7. [x] **Refresh goals when timers change state.** **Impact: Medium** — avoids stale guidance when a hive becomes ready or the wild hive recovers. **Category: Fix.** Goals update when production reaches readiness or tree recovery expires, including visibility transitions. See hive timers and goals in [index.html](./index.html).

8. [x] **Add meaningful apiary milestones.** **Impact: Medium** — gives players reasons to return beyond increasing resource totals. **Category: New Feature.** Optional first-harvest, first-customer and two-hive milestones award one-time resources; progress is shown in settings and persists across saves.

9. [ ] **Profile mobile performance and timer accuracy.** **Impact: Medium** — poor frame rates can affect both responsiveness and live production timing. **Category: Change.** Live production now integrates elapsed time across seasons instead of using capped animation delta; a simulated five-frame-per-second browser check passed. Real-phone profiling remains outstanding.

10. [x] **Clarify or develop colony breeding.** **Impact: Medium** — gives expansion a stronger beekeeping identity and aligns player expectations. **Category: New Feature.** Removed the unsupported breeding claim from [README.md](./README.md); a breeding mechanic remains a future design option.

11. [x] **Consider ambient audio and desktop landscape layout.** **Impact: Low** — can improve atmosphere and desktop comfort once higher-impact issues are addressed. **Category: Change.** After review, retain the current portrait frame and audio rather than adding untested alternatives; landscape and ambience remain future options.
