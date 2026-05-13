# Procedural Distributions

- **Wiki**: https://pzwiki.net/wiki/Procedural_distributions (Build 42.7.0)

## How loot generation works

Containers in a room pick from one or more named distribution lists. Loot generates when the container loads or respawns (per sandbox settings). Each distribution has a `rolls` count; each item entry has a chance value that is **not a weight and not a percentage** — base your values on existing vanilla entries.

Each roll generates a random value influenced by the loot spawn setting and zombie population density, then checks it against each item's chance. The event `OnFillContainer` fires when a container is being filled, allowing you to intercept and modify spawned items.

## Distribution file location

```
ProjectZomboid/media/lua/server/Items/ProceduralDistributions.lua
```

## Distribution table structure

```lua
ProceduralDistributions.list = {
    DistributionName = {
        rolls = 3,
        items = {
            "Item1", 10,   -- item name, chance
            "Item2", 5,
        },
        junk = {           -- ignores zombie density, x1.4 chance multiplier
            rolls = 1,
            items = {
                "Item3", 3,
            }
        }
    },
}
```

### Distribution tags

| Tag                   | Effect                                                                                                                                                       |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `ignoreZombieDensity` | Ignores zombie density impact on spawn chance.                                                                                                               |
| `isShop = true`       | Items spawn at full condition (no degraded state, no bags inside bags). When false: items may spawn with reduced condition, random uses, and stash behavior. |
| `stashChance`         | Chance for the container to be an annotated-map stash.                                                                                                       |
| `canBurn`             | Food can spawn cooked or burnt (25% chance).                                                                                                                 |
| `isWorn`              | Items spawn with reduced condition; clothing can be dirty/bloody/holed.                                                                                      |
| `isTrash`             | Heavier degradation than `isWorn`; clothing is almost certainly dirty.                                                                                       |
| `isRotten`            | Non-rotten food becomes rotten (75% chance) or gains age.                                                                                                    |
| `maxMap`              | Limits the same item to a max count in the container.                                                                                                        |

`isShop` being absent (or false) enables a range of degradation behaviors on spawned items — weapons lose condition, drainable items lose uses, bags can contain items, and the container can become a stash.

## Adding items to an existing distribution

Place your Lua file in `media/lua/server/`. Items from non-`Base` modules must include the module prefix: `MyMod.MyItem`.

The simplest approach works but is verbose at scale:

```lua
table.insert(ProceduralDistributions.list["KitchenCounter"].items, "MyMod.MyKnife")
table.insert(ProceduralDistributions.list["KitchenCounter"].items, 5)
```

A better pattern: declare all additions in one table, then insert them with a loop:

```lua
local myDistribution = {
    KitchenCounter = {
        items = {
            "MyMod.MyKnife", 5,
            "MyMod.MyCup",   8,
        },
        junk = {
            "MyMod.MyNapkin", 3,
        },
    },
    GigamartPots = {
        items = {
            "MyMod.MyPot", 10,
        },
    },
}

local ProceduralDistributions_list = ProceduralDistributions.list
local table_insert = table.insert

local function insertInDistribution(distrib)
    for k, v in pairs(distrib) do
        local dist = ProceduralDistributions_list[k]

        local items = v.items
        if items then
            local dist_items = dist.items
            for i = 1, #items do
                table_insert(dist_items, items[i])
            end
        end

        local junk = v.junk
        if junk then
            local dist_junk = dist.junk
            for i = 1, #junk do
                table_insert(dist_junk, junk[i])
            end
        end
    end
end

insertInDistribution(myDistribution)
```

## Other loot tables

Generic clutter items used by many distributions live in separate files:

```
lua/server/Items/
    Distribution_BinJunk.lua
    Distribution_ClosetJunk.lua
    Distribution_CounterJunk.lua
    Distribution_DeskJunk.lua
    Distribution_ShelfJunk.lua
    Distribution_SideTableJunk.lua
lua/server/Vehicles/
    VehicleDistribution_GloveBoxJunk.lua
    VehicleDistribution_SeatJunk.lua
    VehicleDistribution_TrunkJunk.lua
```

Backpack loot is in `lua/server/Items/Distribution_BagsAndContainers.lua` under the `BagsAndContainers` key.

## Inspecting distributions in-game

Enable **LootZed** in the debug cheats menu (see [debug-mode.md](./debug-mode.md)). It shows each distribution list attached to a container and each item's spawn chance.

## The bloat problem

Adding items to a distribution list increases the average item count in that container type. This is a known systemic issue — popular mods that add large clothing or gun packs to single distribution lists cause containers to overflow.

Two solutions that avoid bloat:

- **Item variants**: create one item script and change its texture, icon, name, and tooltip dynamically (e.g. a single "game card" item that randomizes its face). Avoids adding many entries.
- **Dummy items + OnFillContainer**: add a single dummy item with a normal chance. Intercept `OnFillContainer`, remove the dummy, and randomly replace it with one of your real items. The total number of distribution entries stays at one.

Reducing chance values is a band-aid: it makes your item rarer than similar vanilla items of the same type, breaking consistency.
