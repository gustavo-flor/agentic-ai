# Debug Mode

- **Wiki**: https://pzwiki.net/wiki/Debug_mode (Build 42.18.0)

## Enabling

Add `-debug` to the game's launch options before booting. In Steam: right-click Project Zomboid → Properties → General → Launch Options, then enter `-debug`.

## Debug scenarios

The main menu shows a list of debug scenarios when in debug mode; double-clicking one starts the game in a predefined state.

Custom scenarios can be added by editing or creating files in `ProjectZomboid/media/lua/client/DebugUIs/Scenarios/DebugScenario.lua`.

## Debug menu

A gray bug icon appears on the left of the HUD when in debug mode. Clicking it opens the debug menu.

### Main tab

#### General debuggers

**Game** — general game options: adjust game speed (1–1000), trigger/stop the helicopter event, stop current weather, disable radio/TV broadcasts.

**Blood** — adjust, randomize, or zero blood on any body part.

**Moodles and Body** — sliders and checkboxes for every player stat: hunger, thirst, fatigue, endurance, fitness, drunkenness, pain, panic, stress, temperature, infection level, calories, weight, god mode, invisibility, and more.

**Search Mode** — toggle the foraging search-mode overlay and debug icons that show location, radius, and vision info for foraging items.

#### Cheats

Quick toggles for common developer cheats: Invisible, God Mode, No Clip, Fast Move, Timed Action Instant, Unlimited Carry/Endurance/Ammo, Know All Recipes, Build/Agriculture/Fishing/Mechanics/Moveable cheat modes, and others.

#### Climate debuggers

Access via "Other debuggers":

- **WeatherFX Panel** — tweak fog intensity, precipitation levels and type. Disable the Climate manager first or the daily simulation will override changes.
- **Weather plotter / Daily values** — stock-market-style graphs of weather over hours, days, and months.
- **Thunderbug** — shows storm geography and lightning locations on the map.

#### Player's Stats

Access to a live view of the player's traits and skills.

#### Items List

Spawn any item in the game directly into the player's inventory.

### Dev tab

**isoRegions** — highlights all buildings and detects fully enclosed structures (performance-heavy, may slow the game).

**Zombie Population** — top-down map showing active zombies (yellow), inactive zombies (red), and the player (green). Shows AI cells, building chunks, room borders, and furniture outlines. Zoom and pan with scroll and RMB drag.

**Stash debuggers** — debug annotated-map house stashes.

**Animation Viewer** — preview all game animations.

**Attachment Editor** — adjust 3D model attachment points (rotation, translation, scale per bone).

Additional stub panels: Anim monitor, Zomboid Radio, Chunk Debugger, Global Objects, Map Editor, Vehicle Editor, World Flares, Statistic, GlobalModData.

## Contextual right-click menus

In debug mode, right-clicking adds extra menus for tiles, zombies, objects, and items. Notable entries:

- **Copy tile / Destroy tile** — copy or permanently destroy any tile; a submenu lets you pick when multiple tiles share a cell. Shows the tile ID.
- **Destroy Item** — permanently destroy an item (similar to trash cans).

## Lua Debugger

Open with `F11` (rebindable in options). The debugger breaks at the next Lua entry point and shows all loaded Lua files with the current line highlighted in green.

**Map Debugger** — click "Map" inside the Lua Debugger. Press `T` to teleport to the cursor position; the teleport happens silently after closing the debugger.

**Lua file browser** — find the `.lua` file you're editing, select it, then click "Reload file" to attempt hot-reloading your changes in-game. This is unreliable but useful for quick iteration.

If an option in the debugger causes the client to freeze on launch, set it to `false` in `%UserProfile%/Zomboid/debug-options.init`.

## Lua Console

Also called the **Command Console**, accessible from in-game options. Allows running arbitrary Lua in the live game session. Useful for inspecting globals, calling API functions, and testing snippets without restarting.
