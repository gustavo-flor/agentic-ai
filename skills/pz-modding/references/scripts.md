# Scripts

- **Wiki**: https://pzwiki.net/wiki/Scripts (Build 42.17.0)
- **API reference**: [PZ Scripts Data (ScriptsDocs)](https://pzwiki.net/wiki/PZ_Scripts_Data) — full parameter reference for all script blocks
- **VS Code extension**: [ZedScripts](https://marketplace.visualstudio.com/items?itemName=asledgehammer.zed-script) — syntax highlighting and diagnostics

Scripts (also called "zedscripts") define items, recipes, vehicles, sounds, models, and more as plain-text data files. They are **not** a programming language — the game parses them and caches the definitions in Java. Logic goes in Lua; data goes in scripts.

## File location

Script files use the `.txt` extension and live in `media/scripts/`. Subfolders are allowed. Any `.txt` file in that folder is parsed as a script.

```
media/
└── scripts/
  ├── myItems.txt
  └── subFolder/
    └── myRecipes.txt
```

## Syntax rules

- Block structure uses `{` and `}`.
- Key-value pairs use `Key = Value,` — the trailing comma is mandatory, including on the last line of a block.
- Comments use `/* ... */` (Java multiline style). Single-line `//` comments do not work.
- Rules can vary between script block types; check the specific block docs when in doubt.

## Module block

Every script file (except sandbox options) must open with a `module` block:

```java
module Base {
  ...
}
```

The vanilla module is `Base`. You can use your own:

```java
module MyMod {
  ...
}
```

When using a custom module, always reference your items with the full prefix (`MyMod.MyItem`) to avoid collisions. Without a prefix, the game searches: current module → imported modules → `Base` → everything else. This resolution is inconsistent across block types, so always use the full prefix.

If you put entries in the `Base` module, add a mod-specific prefix to the ID to avoid clashing with other mods: `MyMod_MyItem`.

## Imports block

Importing a module lets you reference its items without a prefix:

```java
module MyMod {
  imports {
    Base
  }
  ...
}
```

Avoid imports in practice — explicit prefixes are safer and clearer.

## Script block reference

| Block                             | Purpose                                                                          |
| --------------------------------- | -------------------------------------------------------------------------------- |
| `item`                            | Items: weapons, clothing, food, containers, literature, and more.                |
| `craftRecipe`                     | Crafting recipes (Build 42).                                                     |
| `Recipe`                          | Crafting recipes (Build 41).                                                     |
| `EvolvedRecipe`                   | Recipes whose output changes based on ingredients (usually food).                |
| `Fixing`                          | How items can be repaired.                                                       |
| `Vehicle`                         | Vehicle definitions (complex, composed of sub-blocks).                           |
| `Model`                           | 3D model bindings (mesh, texture, animations).                                   |
| `Sound`                           | Sound parameter definitions (volume, pitch, etc.).                               |
| `entity`                          | Buildable world objects: walls, fences, furniture (Build 42).                    |
| `Fluid`                           | Fluid definitions (Build 42).                                                    |
| `mannequin`                       | Mannequin types for custom world placement.                                      |
| `template`                        | Reusable script fragments, currently used for vehicle templates.                 |
| `character_profession_definition` | New character professions.                                                       |
| `character_trait_definition`      | New character traits.                                                            |
| `perk`                            | New perk definition (name, category, XP values). Must live in `media/perks.txt`. |
| `Sandbox options`                 | Custom sandbox settings. No module block required.                               |
| `physicsHitReaction`              | Bullet/explosion physics reactions on objects.                                   |
| `ragdoll`                         | Ragdoll bone physics properties.                                                 |
| `animation`                       | Animation definitions.                                                           |
| `Multistage build`                | Multi-stage buildables (Build 41).                                               |

## Overriding scripts

**File override**: naming a file at the same relative path as a vanilla file replaces all definitions in that file. Avoid this — vanilla scripts bundle many items per file, so a file override will conflict with other mods touching the same file.

**Soft override**: redefining a script block that already exists (for block types that support it, like `item`) merges your definition into the existing one rather than replacing it. Only the keys you specify change; unspecified keys keep their vanilla values:

```java
/* only changes MaxWeight, leaves everything else vanilla */
module MyMod {
  item Base.Axe {
    MaxWeight = 5,
  }
}
```

Check the ScriptsDocs for whether a given block type supports soft overrides.

## Minimal item example

```java
module MyMod {
  item MyKnife {
    Type            = Weapon,
    DisplayName     = My Knife,
    Icon            = MyMod_MyKnife,
    Weight          = 0.3,
    MaxRange        = 0.9,
    MinDamage       = 0.5,
    MaxDamage       = 1.0,
    WeaponSprite    = Knife,
  }
}
```

## Minimal recipe example (Build 42)

```java
module MyMod {
  craftRecipe MakeRope {
    Result: MyMod.Rope,
    inputs {
      item[2] Clothing_Rags,
    }
  }
}
```
