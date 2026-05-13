# Rendering

- **Wiki**: https://pzwiki.net/wiki/Rendering (Build 42.13.1)

Rendering generates 2D images from 3D models using software like Blender. In PZ modding it is used for mod presentation art and for in-game media items like fliers (Build 42). It is not a direct gameplay modding discipline but supports the visual side of mod work.

## Getting started

The standard starting point is the two-part tutorial by SQz:

1. [How To Set Up a Character from Project Zomboid in Blender for Animation](https://www.youtube.com/watch?v=31H54TpJjSA) — importing and rigging a PZ character for 3D rendering.
2. [How To Add Further Details to Your Project Zomboid Character in Blender](https://www.youtube.com/watch?v=b5aqvZpMe7o) — building a full PZ-style scene.

## Tiles and 2D sprites

Most vanilla tiles have no 3D model — they are 2D sprites. To render a tile-based scene, you need to reconstruct the 3D shape manually and apply the 2D texture to the UV map. The process is covered in the game asset importing guide linked from [file-formats.md](./file-formats.md).

## Character rigs for posing

Ready-made character rigs for posing are listed on the [Animation rigs wiki page](https://pzwiki.net/wiki/Animation_rigs). Using a rig means you can pose a character without building one from scratch.

## See also

- [Modeling reference](./modeling.md) — 3D model file formats
- [Animation reference](./animation.md) — available rigs and animation workflow
