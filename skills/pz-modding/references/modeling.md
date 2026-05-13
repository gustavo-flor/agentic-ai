# Modeling

- **Wiki**: https://pzwiki.net/wiki/Modeling (Build 42.14.0)

Modeling in PZ means creating 3D models for items, vehicles, or rendering. The community's primary tool is [Blender](https://www.blender.org/), but any software that can export the supported formats works.

## Supported formats

| Format                 | Extension | Notes                                                                                                                  |
| ---------------------- | --------- | ---------------------------------------------------------------------------------------------------------------------- |
| Filmbox FBX            | `.fbx`    | Most common format for models and animations. Supports hot-reloading in-game when the file is updated.                 |
| GL Transmission Format | `.glb`    | Allows animations to be bundled inside the model file. Useful for animating clothing or tiles. Supports hot-reloading. |
| DirectX                | `.x`      | Used by most vanilla assets. Widely unsupported in modern 3D software. Do not use for new mods.                        |

Use `.fbx` or `.glb`. The `.x` format causes more problems than it solves and there is no reason to prefer it over the other two.

## See also

- [Animation reference](./animation.md) — creating and importing custom animations
- [Scripts reference](./scripts.md) — defining items and vehicles that reference your models via `Model` script blocks
- [File formats reference](./file-formats.md) — supported model formats and the importing-assets workflow
