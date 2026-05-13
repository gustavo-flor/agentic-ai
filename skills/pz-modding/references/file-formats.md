# File Formats

- **Wiki**: https://pzwiki.net/wiki/File_formats (Build 42.14.0)

## Programming formats

- **Lua** is the primary scripting language for mods. PZ uses Kahlua, a Java implementation of Lua 5.1, which allows Lua scripts to call exposed Java classes and methods. See the [Lua API](./lua-api.md) reference.
- **XML** is used for clothing items and animation definitions via AnimSets.
- **Text files (.txt)** are used for scripts and translations. The internal formatting varies by file type. The [Zed Script](https://marketplace.visualstudio.com/items?itemName=asledgehammer.zed-script) addon for VS Code provides syntax support for both.

## Textures and images

The game uses 8-bit PNGs for textures and images (16-bit is not accepted). Most inventory icons and UI elements are stored in `.pack` files rather than loose PNGs.

### Icon files

Item icons are referenced in scripts by name (e.g. `Icon = MyItemIcon`). The game prefixes `item_` when searching for the file, so the actual filename must be `item_MyItemIcon.png`. Loose icons go in `media/textures/`, not subfolders, unless you mirror the prefix in the folder name.

For a script icon value of `mySubFolder/MyItemIcon`, the file must be at:

```
media/
└── textures/
    └── item_mySubFolder/
        └── MyItemIcon.png
```

Vanilla icons live in `media/texturepacks/UI2.pack` and are not loose files.

### TexturePacks (.pack files)

Pack files are texture atlases that bundle many images into a single PNG with an index of coordinates. They live in `media/texturepacks/` (e.g. `Characters.pack`, `Tiles.pack`, `UI.pack`). Use [Pack Viewer](https://pzwiki.net/wiki/Pack_Viewer) to inspect them.

The binary format (all integers are little-endian):

```
int32   number of pages
--- per page ---
int32   length of page name
char[]  page name
int32   number of entries
int32   mask (boolean flag, purpose unclear, safe to skip)
--- per entry ---
int32   length of entry name
char[]  entry name
int32   x      (offset within atlas PNG)
int32   y
int32   w      (width on atlas)
int32   h      (height on atlas)
int32   ox     (x offset within full image bounds)
int32   oy     (y offset within full image bounds)
int32   fx     (full image width, e.g. 32 for inventory icons)
int32   fy     (full image height)
--- after entries ---
byte[]  raw PNG data, terminated by: 49 45 4E 44 AE 42 60 82 EF BE AD DE
```

## Modeling and animation formats

Supported 3D model formats:

- `.fbx` (FBX) - recommended
- `.glb` (glTF binary) - recommended
- `.x` (DirectX) - not recommended, mostly outdated

Most 3D software does not support `.x` natively. Use Blender or other tools with appropriate import addons. See the [Importing assets](https://pzwiki.net/wiki/Importing_assets) wiki page for tooling details.

**Video** uses the `.bik` (Bink) format. Can only be installed manually.

## Map formats

### map_sand.bin

Stores sandbox settings as a contiguous array of big-endian 32-bit integers. Values map to game options in this order:

```
Zombies, Distribution, Survivors, Speed, DayLength, StartMonth, StartTime,
WaterShut, ElecShut, Loot, Temperature, Rain,
ZombieStats: Speed, Strength, Toughness, Transmission, Mortality, Reanimate,
             Cognition, Memory, Decomposition, Sight, Hearing, Smell
```

Note: if the world version (from `map_ver.bin`) is below 5, omit Temperature and Rain. Useful for tweaking sandbox options after world creation.

### .lotheader files

Paired with `.lotpack` files in `media/maps/<MapName>/`. Each `world_X_Y.lotpack` has a corresponding `X_Y.lotheader`. All integers are little-endian.

Structure:

```
int32       format version (0 as of build 12)
int32       number of cached tiles
string[]    tile names, newline (0x0A) delimited, read byte by byte
byte        skip (padding byte after tile strings)
int32       lot width
int32       lot height
int32       number of levels
int32       number of rooms
--- per room ---
string      room name (0x0A delimited)
int32       floor level
int32       number of rectangles
  --- per rectangle ---
  int32     x  (+ wX * 300)
  int32     y  (+ wY * 300)
  int32     width
  int32     height
int32       number of objects
  --- per object ---
  int32     object type
  int32     x  (+ wX * 300)
  int32     y  (+ wY * 300)
int32       number of buildings
--- per building ---
int32       number of rooms
int32[]     room indexes
byte[30][30] zombie density grid (reads to end of file)
```
