# Mapping

- **Wiki**: https://pzwiki.net/wiki/Mapping (Build 42.16.1)

Mapping in PZ means creating new map cells — 300×300 tile areas (Build 41) or 256×256 (Build 42, though the game mixes both inconsistently). Replacing an existing vanilla cell requires recreating the entire cell including terrain and all buildings; you cannot drop a single building into an existing cell without remaking everything around it.

Browse the full Knox Country layout at the [Project Zomboid Map Project](https://map.projectzomboid.com/).

## Tools

Official tools for Build 42 are not yet released. Currently available:

- **[Mapping tools (Alree)](<https://pzwiki.net/wiki/Mapping_tools_(Alree)>)** — first unofficial Build 42 mapping tool.
- **[Mapping tools (Community Edition)](<https://pzwiki.net/wiki/Mapping_tools_(Community_Edition)>)** — forks the official tools, merges Alree's improvements, and adds more on top. Recommended for Build 42.
- **[Mapping tools (official)](<https://pzwiki.net/wiki/Mapping_tools_(official)>)** — only covers Build 41. Official Build 42 versions are in development and will release alongside B42 stable.

## Folder structure

Map files live in `media/maps/<MapName>/` and are produced by exporting from the mapping tools:

```
media/
└── maps/
    └── MyMap/
        ├── map.info
        ├── objects.lua
        ├── spawnpoints.lua
        ├── thumb.png
        ├── worldmap.xml
        ├── worldmap-forest.xml
        ├── X_Y.lotheader
        ├── chunkdata_X_Y.bin
        └── world_X_Y.lotpack
```

`MyMap` must be a unique name. Key files:

| File                  | Purpose                                                                                       |
| --------------------- | --------------------------------------------------------------------------------------------- |
| `map.info`            | Map name, description, cell coordinates, and other metadata.                                  |
| `X_Y.lotheader`       | Defines the cell location. See [file-formats.md](./file-formats.md) for the binary structure. |
| `world_X_Y.lotpack`   | Packed tile data for cell X,Y.                                                                |
| `chunkdata_X_Y.bin`   | Chunk data for the cell.                                                                      |
| `objects.lua`         | Car spawn zones and navigation mesh data.                                                     |
| `spawnpoints.lua`     | Player spawn point XYZ coordinates per occupation.                                            |
| `thumb.png`           | Thumbnail shown on the spawn selection screen.                                                |
| `worldmap.xml`        | World map overlay data (streets, labels).                                                     |
| `worldmap-forest.xml` | Forest biome overlay for the world map.                                                       |

## Building pools and tile packs

The community maintains ready-to-use building collections and tile packs that can be dropped into your map mod. See the wiki for the full lists:

- [Building pools](https://pzwiki.net/wiki/Mapping#Building_pools) — official and community building sets
- [Tile packs](https://pzwiki.net/wiki/Mapping#Tile_packs) — Workshop addons with additional tiles

## Learning resources

- [Zomboid Map Making Quick Guide](https://www.youtube.com/playlist?list=PL9c7w_YTJlV_R0mgP-Rwb1i56SudS30XV) by DystopianOutcasts_Rax — recommended playlist
- [Daddy Dirkie Dirk's mapping tutorials](https://www.youtube.com/playlist?list=PLc7_sQ-Tb6e3siC4Cwu4JfxuqJZkGO6sy) — the most cited tutorial series; made for Build 41 but still valid for Build 42
- [Official TIS Mapping guides](https://pzwiki.net/wiki/TIS_Modding_Guides#Mapping_guides) — official guides by The Indie Stone
- [Unofficial Mapping Discord](https://pzwiki.net/wiki/Unofficial_Mapping_Discord)
