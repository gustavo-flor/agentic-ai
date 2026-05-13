# Mod Structure

- **Wiki**: https://pzwiki.net/wiki/Modding_structure (Build 42.14.0)

## Where mods live

The cache folder has two locations where PZ recognizes local mods:

- `Zomboid/mods/` - manual install folder, not recommended for development
- `Zomboid/Workshop/` - development and Steam Workshop upload folder, use this one

Mods with the same Mod ID in multiple folders will clash and overwrite each other silently. Never have two copies of the same mod active at once. Also avoid being subscribed to your own mod on the Workshop while developing locally.

Downloaded Workshop mods land in `Steam/steamapps/workshop/content/108600/<WorkshopID>/` — do not work from there.

## Workshop folder structure

```
Zomboid/
└── Workshop/
    └── MyExampleMod/
        ├── workshop.txt       (upload metadata)
        ├── preview.png        (256x256, shown on Steam Workshop page)
        └── Contents/
            └── mods/
                ├── MyMod1/
                │   └── ...
                └── MyMod2/
                    └── ...
```

Only the `Contents/` folder gets uploaded to the Workshop. Everything else inside `MyExampleMod/` (images, assets, `.git/`, `.vscode/`, `README.md`) is ignored by the game and not uploaded. This makes `MyExampleMod/` a good root for your git repository.

## Mod folder structure (Build 42)

Build 42 introduced versioning folders and a mandatory `common/` folder. The `mod.info` and `poster.png` sit inside each versioning folder, not at the mod root.

```
Contents/
└── mods/
    └── MyMod1/
        ├── common/            (mandatory, even if empty)
        │   └── media/
        │       └── ...
        ├── 42/
        │   ├── media/
        │   │   └── ...
        │   ├── mod.info
        │   └── poster.png
        └── 42.1/              (optional, for version-specific overrides)
            ├── media/
            │   └── ...
            ├── mod.info
            └── poster.png
```

The game loads `common/` first, then the closest versioning folder to the running game version (which overwrites any files also present in `common/`). At least one of the two must exist for the mod to be recognized.

### Versioning folder naming

Minor version is not supported in the folder name — `42.1.5` is treated as `42.1`. Examples:

```
42       => 42.0
42.12    => 42.12
42.1.5   => 42.1
43.5.1   => 43.5
```

Put large static assets (models, textures, animations) in `common/` so they are not duplicated across version folders. Put `mod.info`, Lua scripts, and anything that changes between game versions in versioning folders.

## Build 41 vs Build 42

Build 41 mods place `media/` directly under the mod folder (no `common/` or versioning folders). Both structures can coexist in the same mod folder since the extra nesting of Build 42 keeps them isolated:

```
Contents/mods/MyMod1/
    ├── media/      (Build 41)
    ├── common/     (Build 42)
    └── 42/         (Build 42)
```

## media/ subfolder reference

| Subfolder               | Used for                                    |
| ----------------------- | ------------------------------------------- |
| `lua/client/`           | client-side Lua scripts                     |
| `lua/server/`           | server-side Lua scripts                     |
| `lua/shared/`           | Lua loaded on both client and server        |
| `lua/shared/Translate/` | translation files                           |
| `scripts/`              | item, recipe, vehicle script definitions    |
| `clothing/`             | clothing item XML definitions               |
| `textures/`             | PNG texture files (can use subfolders)      |
| `ui/`                   | PNG images for UI elements                  |
| `models_X/`             | `.fbx` or `.x` model files                  |
| `anims_X/`              | animation files                             |
| `AnimSets/`             | animation trigger/parameter XML definitions |
| `maps/`                 | map cells and assets                        |
| `sound/`                | `.ogg` or `.wav` audio files                |

Files in `textures/`, `ui/`, `models_X/`, and `sound/` can be placed in subfolders and will replace vanilla files at the same relative path.

Note: Linux and macOS are case-sensitive. Use exactly the casing shown above or mods will fail to load on those systems.

## mod.info

The `mod.info` file is what makes the game recognize a mod. It lives inside the versioning folder (e.g. `42/mod.info`). See the [mod.info wiki page](https://pzwiki.net/wiki/Mod.info) for the full field reference.

## workshop.txt

Required for the in-game uploader. Lives at `MyExampleMod/workshop.txt` (not inside `Contents/`). See the [workshop.txt wiki page](https://pzwiki.net/wiki/Workshop.txt) for fields.
