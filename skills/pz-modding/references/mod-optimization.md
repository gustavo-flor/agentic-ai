# Mod Optimization

- **Wiki**: https://pzwiki.net/wiki/Mod_optimization (Build 42.10.0)
- **External guide**: [Lua optimization guide by Albion](https://github.com/demiurgeQuantified/PZModdingGuides/blob/main/guides/Optimisation.md)

## Core rule: don't run what you don't need to

Every optimization below is a variation of the same principle: if code doesn't need to run, don't run it. This matters most in high-frequency events like `OnZombieUpdate` and `OnTick`, which run thousands of times per second.

## Lua optimizations

### Guard against no-ops

Before mutating state, check whether the change is actually needed. A single cheap check can eliminate dozens of Java calls:

```lua
-- bad: calls setSkeleton every tick for every zombie
local function OnZombieUpdate(zombie)
    zombie:setSkeleton(true)
end

-- better: skips zombies already set
local function OnZombieUpdate(zombie)
    if not zombie:isSkeleton() then
        zombie:setSkeleton(true)
    end
end
```

Exception: if the setter is cheaper than the getter (e.g. a simple field assignment vs. a comparison), calling unconditionally can be faster. Decompile the Java to check.

### Use local variables

Global variable lookups are more expensive than local ones. Never use globals by default. When you must access a vanilla global, cache it into a local once at file scope:

```lua
-- bad
MyFunctions = {}

-- better
local MyFunctions = {}

-- caching a vanilla global
local ProceduralDistributions = ProceduralDistributions
```

### Cache aggressively

Avoid calling the same function multiple times when you can store the result. This applies both inside functions and at file scope for objects that never change across a save:

```lua
-- bad: calls getPrimaryHandItem() 6 times
local function OnPlayerUpdate(player)
    if player:getPrimaryHandItem() and instanceof(player:getPrimaryHandItem(), "HandWeapon") then
        if player:getPrimaryHandItem():isRanged() then
            if player:getPrimaryHandItem():getCurrentAmmoCount() ~= player:getPrimaryHandItem():getMaxAmmo() then
                player:getPrimaryHandItem():setCurrentAmmoCount(player:getPrimaryHandItem():getMaxAmmo())
            end
        end
    end
end

-- better: 6 Java calls instead of 13
local function OnPlayerUpdate(player)
    local weapon = player:getPrimaryHandItem()
    if weapon and instanceof(weapon, "HandWeapon") and weapon:isRanged() then
        local maxAmmo = weapon:getMaxAmmo()
        if weapon:getCurrentAmmoCount() ~= maxAmmo then
            weapon:setCurrentAmmoCount(maxAmmo)
        end
    end
end
```

Cache stable objects at file scope rather than retrieving them on every call:

```lua
-- bad: fetches zombie list every tick
Events.OnTick.Add(function()
    local zombieList = getCell():getZombieList()
    print(zombieList:size())
end)

-- better: fetch once when the cell loads
local zombieList
Events.OnPostMapLoad.Add(function(cell)
    zombieList = cell:getZombieList()
end)
Events.OnTick.Add(function()
    print(zombieList:size())
end)
```

### Minimize function calls

Function call overhead in Kahlua is significant. In hot paths, inline operations rather than calling helper functions, including Lua's `math` library:

```lua
-- prefer inline equivalents over math.*
local max   = value > maxvalue and value or maxvalue
local min   = value < minvalue and value or minvalue
local pow   = value ^ exponent
local sqrt  = value ^ 0.5
local floor = value - value % 1
local abs   = value < 0 and -value or value
```

### Remove print statements before publishing

`print()` is extremely expensive. Leaving prints in published mods causes lag proportional to how often they fire — and with many mods loaded, prints that each fire every in-game hour combine into a single lag spike every in-game hour. Remove all prints before uploading. Do not gate them behind a debug-mode check (that just runs extra code for no user benefit).

### Use proper arrays

When building integer-keyed lists, use `table.newarray()` instead of `{}`. Proper arrays are faster to iterate and signal intent clearly:

```lua
-- bad
local myArray = { "entry1", "entry2", "entry3" }

-- better
local myArray = table.newarray("entry1", "entry2", "entry3")
```

`table.newarray()` arrays cannot be saved (no mod data, no network commands).

### Prefer numeric loops over `ipairs`/`pairs`

`ipairs` and `pairs` add function-call overhead. Use numeric for-loops for arrays:

```lua
-- slower
for i, v in ipairs(t) do print(v) end

-- faster
for i = 1, #t do
    local v = t[i]
    print(v)
end
```

For key tables, combine a lookup table with a `table.newarray` of keys and iterate the array numerically. Only worth the complexity in performance-critical loops.

### Use `newrandom()` instead of `ZombRand`

`ZombRand` generates high-quality randomness that is rarely necessary. `newrandom()` is cheaper:

```lua
local myRandom = newrandom()
local value = myRandom:random(min, max)
```

### Load balancing

Spreading work across multiple ticks prevents single-frame spikes. Instead of processing every zombie every tick, process one (or N) per tick:

```lua
-- credit: Albion
local zombieList
Events.OnGameStart.Add(function()
    zombieList = getPlayer():getCell():getZombieList()
end)

local zeroTick = 0
Events.OnTick.Add(function(tick)
    local zombieIndex = tick - zeroTick
    if zombieList:size() > zombieIndex then
        local zombie = zombieList:get(zombieIndex)
        -- process this zombie
    else
        zeroTick = tick + 1
    end
end)
```

Apply the same pattern to any queued action: add work to a list and drain N items per tick.

### Benchmarking

Use `GameTime.getServerTime()` (returns nanoseconds) to measure how long code takes:

```lua
GameTime.setServerTimeShift(0)
local getTime = GameTime.getServerTime

local start = getTime()
-- code to measure
local delta = getTime() - start
```

## Modeling and texturing

The isometric camera is almost always zoomed out. High-resolution textures and high polygon counts provide no visible benefit and hurt performance, especially when mods stack.

### Texture size limits

Follow The Indie Stone's official recommendations:

| Asset type        | Max texture size              |
| ----------------- | ----------------------------- |
| Vehicles          | 512x512                       |
| Body and clothing | 256x256                       |
| Weapons           | 128x128 (varies by item size) |
| Hats              | 128x128                       |

Oversized textures are one of the most common causes of memory leaks and performance degradation in modded saves.

### Polycount

Keep polygon counts in line with vanilla equivalents. Small accessories (earrings, buttons) need almost no geometry — use the texture to add visual detail instead.

### UV mapping

Pack UVs tightly to minimize wasted atlas space. Less empty space means a smaller texture file and better texturing resolution per polygon.
