# Game Files

- **Wiki**: https://pzwiki.net/wiki/Game_files (Build 42.12.3)

## Accessing the game folder

The game folder holds most of the game code, files, textures, maps, models, etc. The easiest way to open it via Steam is right-clicking Project Zomboid > Manage > Browse local files (or Properties > Installed files > Browse).

The game folder path looks like:

```
Steam/
└── steamapps/
    └── common/
        └── ProjectZomboid/
            └── ...
```

## Cache folder

The cache folder (named `Zomboid/` by default) holds game configuration, local mods, and save data. It is not deleted when you uninstall the game. On Windows it lives at:

```
%UserProfile%/
└── Zomboid/
    └── ...
```

You can override its location with the `-cachedir=<path>` startup parameter. If the path doesn't exist the game will create it. Deleting the folder can fix some issues but will erase settings and saves, so back them up first.

### Logs folder (console.txt)

The console file is the main game log. It contains errors, warnings, and general runtime info. `print()` calls from Lua show up here. Created fresh each launch at:

```
%UserProfile%/
└── Zomboid/
    └── console.txt
```

This is the first place to check when debugging a mod.

## Media folder

Most modding-relevant files live under the `media/` subfolder of the game install:

```
Steam/
└── steamapps/
    └── common/
        └── ProjectZomboid/
            └── media/
                └── ...
```

This is where scripts, Lua files, textures, sounds, maps, and models are kept. Mods mirror this structure inside their own `media/` folder.

## Java files

The game's Java source (compiled) lives in the `zombie/` folder:

```
Steam/
└── steamapps/
    └── common/
        └── ProjectZomboid/
            └── zombie/
                └── ...
```

The `.class` files here are not human-readable as-is. Use a decompiler (Fernflower, CFR, Procyon) to get readable Java. See the [Java modding](./references/java-modding.md) reference for details.

## Searching the game files

Open the `media/` folder (or the decompiled `zombie/` output) as a workspace in an IDE to search across all files. Recommended tools:

- **Visual Studio Code** - lightweight, free, easy to set up, good addon support for PZ modding
- **IntelliJ IDEA** - more powerful for navigating Java code, better for Java modding work

Searching by symbol name (e.g. `ISTakeFuel`) across the entire workspace is the standard way to find where something is defined or used in the game code.
