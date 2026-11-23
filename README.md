Honeybee Simulator 🐝🍯

Created by a real-life beekeeper from Coorg (Kodagu), Honeybee Simulator is a cozy beekeeping and farm management game where you build, care for, and expand your own apiary. Gather materials, craft bee boxes, tend to your colonies, protect them from pests, and harvest golden honey at your own pace. After storing your first honey, the couple plans a shop at the entrance. Gather 10 wood and 10 scrap to build it, then enter the shop to meet customers face to face.

For a local preview, run `python3 -m http.server 8000` from this folder and open `http://localhost:8000/`. Some browsers block the soundtrack when the game is opened directly as a `file://` URL.

Standalone model, texture, sound and UI reference exports are organized in [assets/](assets/README.md). The game continues to use its procedural assets.

Move with WASD or the arrow keys, or drag the on-screen joystick on a touch device. Tap or click action prompts, or use Tab and Enter/Space to activate prompts, dialogue, and menu buttons. Escape closes the workbench or settings. Wood and scrap are collected automatically when you walk nearby. Browser pinch zoom is supported.

Use Quit in Settings to save and return to the title screen.

Customers wait at the entrance gate until you enter the shop, then come to the counter one at a time. Susmita is first; the three introductory customers each order one honey. Leave and harvest more if needed; completed introductory sales are saved. Existing shops keep their progress and can meet these customers too. Later customers wait without a timer, and their orders earn tips and reputation. You can also skip later orders or quick-sell honey in the shop.

After mounting your first hive, the tutorial points out the honey cupboard. Optional goals guide your first harvest, storage, shop and sale, then suggest preparing another colony. You can still pursue other activities at your own pace.

During the opening tutorial, green trails and highlights point toward the current objective. After placing the ladder and Bee Box, try the wild hive: the first unsmoked attempt leads to collecting coconut husk and a tin can, then crafting a crude smoker at the workbench. Existing saves keep their smoker access.

The settings menu lists optional milestones. Your first managed-hive harvest awards 4 wood, your first customer order awards 4 scrap, and mounting two populated hives awards 5 coins. Rewards are granted once and saved automatically; milestones already completed in older saves are marked complete without retroactive rewards. Hive production and Queen recovery also continue while the game is closed.

