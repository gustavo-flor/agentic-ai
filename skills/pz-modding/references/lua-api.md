# Lua API

- **Wiki**: https://pzwiki.net/wiki/Lua_(API) (Build 42.17.0)
- **JavaDocs**: https://projectzomboid.com/modding/ — authoritative reference for all exposed Java classes and methods
- **LuaDocs** (unofficial): acts like JavaDocs but organized for Lua-side usage

PZ uses **Kahlua**, a Java implementation of Lua 5.1. Lua scripts call exposed Java classes and methods directly. Every Java call has more overhead than a pure Lua call — see [mod-optimization.md](./mod-optimization.md) for the implications.

## Folder structure and load order

Lua files go in `media/lua/` in one of three subfolders:

| Folder    | Singleplayer | MP client | MP server                           |
| --------- | ------------ | --------- | ----------------------------------- |
| `client/` | loaded       | loaded    | NOT loaded                          |
| `server/` | loaded       | loaded    | loaded (only when a save is active) |
| `shared/` | loaded       | loaded    | loaded                              |

Put client-only UI and rendering code in `client/`, server-authoritative logic in `server/`, and anything both sides need in `shared/`. Files with the same path as vanilla files override them — use unique names and nest files in a subfolder named after your mod to avoid collisions with other mods.

Load order across mods: shared vanilla → shared mods → client vanilla → client mods. Server files load when a save launches, in the same vanilla-then-mods order.

## The event system

The primary way to run code at the right moment is subscribing to a named event:

```lua
local function OnZombieUpdate(zombie)
    -- zombie is an IsoZombie instance
end

Events.OnZombieUpdate.Add(OnZombieUpdate)
```

Commonly used events:

| Event                  | When it fires                                              |
| ---------------------- | ---------------------------------------------------------- |
| `OnGameStart`          | Once when a save loads                                     |
| `OnPostMapLoad`        | After the cell loads (good for caching cell-level objects) |
| `OnTick`               | Every tick (very frequent — keep handlers cheap)           |
| `OnPlayerUpdate`       | Every tick per player                                      |
| `OnZombieUpdate`       | Every zombie update tick per zombie                        |
| `OnFillContainer`      | When a container's loot is being generated                 |
| `OnServerCommand`      | Client receives a custom command from the server           |
| `OnClientCommand`      | Server receives a custom command from a client             |
| `OnNewSurvivorCreated` | New character is created                                   |

For the full event list see the [events category on the wiki](https://pzwiki.net/wiki/Category:Lua_events).

## Calling Java from Lua

**Static methods** use dot notation (no instance needed):

```lua
local players = IsoPlayer.getPlayers()
```

**Instance methods** use colon notation (`self` passed implicitly):

```lua
local player = getPlayer()           -- LuaManager.GlobalObject global
local speed   = player:getMoveSpeed()
```

**Constructors** use `.new(args...)`:

```lua
local zombie = IsoZombie.new(getCell())  -- creates an instance, not an actual in-game zombie
```

`LuaManager.GlobalObject` methods are exposed as plain globals: `getPlayer()`, `getCell()`, `getWorld()`, `newrandom()`, `instanceof()`, etc.

**Public static fields** of exposed classes are accessible in a global table of the same name:

```lua
local musicName = IsoPlayer.DEATH_MUSIC_NAME
```

**Instance fields** are not directly accessible. Access them via reflection helpers or the [Starlit Library](<https://pzwiki.net/wiki/Starlit_Library_(Modding_project)>) which provides a clean field-access API.

## Class hierarchy and inheritance

Java classes form an inheritance tree. `IsoZombie` inherits from `IsoGameCharacter`, which inherits from `IsoMovingObject`, and so on. All parent class methods are available on the instance. The JavaDocs show the full tree for each class.

Only `public` members are exposed to Lua. `private` and `protected` members require reflection to access.

## Hooking Java methods (Lua side only)

You can intercept Java method calls made from Lua (not Java-to-Java calls):

```lua
local index = __classmetatables[ClassName.class].__index

local original = index.methodName
function index:methodName(...)
    return original(self, ...)
end
```

This does not fire when the method is called internally from Java.

## Lua objects (ISBaseObject)

PZ defines its own class system in Lua on top of `ISBaseObject`. UI panels, timed actions, and many game systems use this. Derive a class with:

```lua
MyClass = ISBaseObject:derive("MyClass")

function MyClass:new()
    local o = ISBaseObject.new(self)
    setmetatable(o, self)
    self.__index = self
    return o
end
```

`ISBaseTimedAction` is the base for all timed actions. UI panels extend `ISPanel`.

## Client-server communication

Send a custom command from client to server:

```lua
sendClientCommand(player, "MyMod", "myCommand", { key = value })
```

Receive it server side via `OnClientCommand`:

```lua
Events.OnClientCommand.Add(function(module, command, player, args)
    if module == "MyMod" and command == "myCommand" then
        -- handle
    end
end)
```

The reverse uses `sendServerCommand` and `OnServerCommand`.

## Useful globals

| Global                         | Description                                            |
| ------------------------------ | ------------------------------------------------------ |
| `getPlayer()`                  | Local `IsoPlayer` instance                             |
| `getCell()`                    | Current `IsoCell`                                      |
| `getWorld()`                   | `GameWorld` instance                                   |
| `getActivatedMods()`           | List of active mod IDs                                 |
| `instanceof(obj, "ClassName")` | Type check for Java objects                            |
| `newrandom()`                  | Cheap random number generator (prefer over `ZombRand`) |
