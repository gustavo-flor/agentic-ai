# Animation

- **Wiki**: https://pzwiki.net/wiki/Animation (Build 42.15.0)

Animation in PZ means creating custom skeletal animations (or replacing existing ones), exporting them in a supported format, and wiring them up via AnimNodes so the game knows when and how to play them.

## File formats

| Format                 | Extension | Notes                                                                                                                             |
| ---------------------- | --------- | --------------------------------------------------------------------------------------------------------------------------------- |
| GL Transmission Format | `.glb`    | Recommended. Lighter file sizes; requires the [Paddlefruit rig](https://pzwiki.net/wiki/Paddlefruit_rig). Supports hot-reloading. |
| Filmbox FBX            | `.fbx`    | Compatible with older rigs; significantly larger file sizes. Supports hot-reloading.                                              |
| DirectX                | `.x`      | Used by vanilla assets. Unsupported in modern software. Do not use for new mods.                                                  |

## Folder structure

Animation-related files span three folders under `media/`:

```
media/
├── anims_X/           animation files (.glb, .fbx, .x)
├── AnimSets/          AnimSet XML files (state machines)
└── actiongroups/      ActionGroup XML files (transition conditions)
```

**AnimSet** — a collection of AnimStates associated with an entity (player, zombie, animal). Each AnimState represents a state the entity can be in (walking, idle, attacking).

**AnimNode** — defined inside an AnimState; specifies which animation plays and under what conditions (e.g. play a different walk animation when injured).

**ActionGroup** — paired with an AnimSet; contains ActionStates that define transition conditions between AnimStates.

## Cow example structure

The cow's animation files give a representative picture of how the folders map to entity behaviors:

```
media/
├── actiongroups/cow/   (attack, death, eating, idle, walk, ...)
├── anims_X/Cow/        (animation files)
└── AnimSets/cow/       (attack, deadbody, death, eating, idle, walk, ...)
```

Every entity that uses custom animations follows this same three-folder pattern.

## Available rigs

Character rigs ready for animation work are listed on the [Character rigs wiki page](https://pzwiki.net/wiki/Character_rigs). Use the Paddlefruit rig for `.glb` export.

## Step-by-step guide

See the [Creating custom animations](https://pzwiki.net/wiki/Creating_custom_animations) wiki page for the full workflow.

## External tutorials

- [How To Create an Animation](https://steamcommunity.com/sharedfiles/filedetails/?id=3035712003) by Dislaik — step-by-step Steam guide.

## See also

- [Modeling reference](./modeling.md) — 3D model formats
- [File formats reference](./file-formats.md) — format details and hot-reloading
