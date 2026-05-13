---
name: pz-modding
description: Expert guide for modding Project Zomboid (PZ). Use this skill whenever the user asks about creating, editing, or debugging a PZ mod, including item scripts, Lua scripting, Java patching, map making, translations, file formats, mod structure, debug mode, startup parameters, loot distributions, mod optimization, 3D modeling, animation, or rendering. Trigger even if the user doesn't say "mod" explicitly but is clearly working inside the PZ modding ecosystem (e.g. "how do I add a new item to PZ", "my Lua script isn't running", "how do I set up a PZ map", "how do I add a custom animation").
---

# Project Zomboid Modding

Project Zomboid modding spans several distinct disciplines. This skill covers the full modding surface for Build 42: from simple script edits that require no programming, to Lua scripting, Java patching, map creation, and translation work.

- **Wiki**: https://pzwiki.net/wiki/Modding (Build 42.17.0)

## Reference Catalog

Use this table to decide which reference file to load for a given task.

| Topic                                                                | When to use                                                                                                                             |
| -------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| [Game files](./references/game-files.md)                             | Understanding PZ's install directory layout, where vanilla assets live, which folders to look in when reverse-engineering game behavior |
| [File formats](./references/file-formats.md)                         | Working with `.pack`, `.lot`, `.world`, `.tmx`, tile definition files, or any other PZ-specific format                                  |
| [Mod structure](./references/mod-structure.md)                       | Setting up a new mod, understanding `mod.info`, folder conventions, load order, and how PZ discovers mods                               |
| [Debug mode](./references/debug-mode.md)                             | Running the game in developer mode, using the in-game debug UI, inspecting objects, testing Lua live                                    |
| [Startup parameters](./references/startup-parameters.md)             | Launch flags for the PZ executable (e.g. `-debug`, `-cachedir`, dedicated server flags)                                                 |
| [Mod optimization](./references/mod-optimization.md)                 | Improving mod performance: event hygiene, avoiding expensive per-tick calls, memory management                                          |
| [Procedural distributions](./references/procedural-distributions.md) | Adding items to container loot tables via `ProceduralDistributions.lua` and `SuburbsDistributions.lua`                                  |
| [Scripts](./references/scripts.md)                                   | Defining items, recipes, vehicles, and clothing via plain-text `.txt` script files, no Lua required                                     |
| [Lua API](./references/lua-api.md)                                   | Hooking into game events, using PZ's Lua globals, writing Lua-side game logic with Kahlua                                               |
| [Java modding](./references/java-modding.md)                         | Patching or extending PZ's Java source, decompiling with tools like IDEA/Fernflower, injecting compiled classes                         |
| [Modeling](./references/modeling.md)                                 | Creating 3D models for items or vehicles, supported formats (`.fbx`, `.glb`), tool recommendations                                      |
| [Animation](./references/animation.md)                               | Creating custom animations, AnimSet/AnimNode/ActionGroup structure, rig selection, hot-reloading                                        |
| [Rendering](./references/rendering.md)                               | Generating art from 3D models for mod presentation or in-game fliers, Blender setup, character rigs for posing                          |
| [Mapping](./references/mapping.md)                                   | Creating new maps or tiles with WorldEd / TileZed, cell layout, integrating custom maps with Knox Country                               |
| [Translation](./references/translation.md)                           | Adding or updating translation strings, `Translate/` folder layout, encoding requirements                                               |

Load only the reference files relevant to the current task.
