# Translation

- **Wiki**: https://pzwiki.net/wiki/Translations (Build 42.16.3)
- **JSON schemas**: [PZ Translation Data repository](https://github.com/pz-wiki-modding/pz-translation-data) — schemas for validating each translation file type

## File location

Translation files live inside `media/lua/shared/Translate/<LanguageCode>/`:

```
media/lua/shared/Translate/
└── EN/
    ├── ItemName.json
    ├── UI.json
    ├── ContextMenu.json
    └── ...
```

As of Build 42.15.0, translation files are `.json` and do not include the language code in the filename. All languages use UTF-8 encoding from 42.15.0 onward.

## Translation types

Each translation type has a specific filename and key prefix. Use `getText(key)` to retrieve most of them at runtime.

| Type                | Filename            | Key prefix         | Notes                                                         |
| ------------------- | ------------------- | ------------------ | ------------------------------------------------------------- |
| `ItemName`          | `ItemName`          | (none)             | Key is the item's full type, e.g. `Base.Axe`.                 |
| `Recipes`           | `Recipes`           | (none)             | Key is the `craftRecipe` block ID.                            |
| `EvolvedRecipeName` | `EvolvedRecipeName` | (none)             | Evolved recipe names.                                         |
| `ContextMenu`       | `ContextMenu`       | `ContextMenu_`     | Right-click menu entries.                                     |
| `IGUI`              | `IG_UI`             | `IGUI_`            | In-game HUD and menus.                                        |
| `UI`                | `UI`                | `UI_`              | General UI elements.                                          |
| `Tooltip`           | `Tooltip`           | `Tooltip_`         | Tooltip strings.                                              |
| `Sandbox`           | `Sandbox`           | `Sandbox_`         | Custom sandbox option labels.                                 |
| `Entity`            | `Entity`            | `EC_`              | Entity script translations.                                   |
| `Fluids`            | `Fluids`            | `Fluid_Name_`      | Fluid names and UI.                                           |
| `Moodles`           | `Moodles`           | `Moodles_`         | Moodle names and descriptions.                                |
| `Moveables`         | `Moveables`         | (none)             | Moveable tile display names.                                  |
| `Farming`           | `Farming`           | `Farming_`         | Farming menu strings.                                         |
| `Mod`               | `Mod`               | (none)             | Keys are `name` and `description` (for mod.info translation). |
| `SurvivorNames`     | `SurvivorNames`     | (none)             | Random name pool for characters.                              |
| `RecipeGroups`      | `RecipeGroups`      | `RecipeGroup_`     | Recipe group names.                                           |
| `MultiStageBuild`   | `MultiStageBuild`   | `MultiStageBuild_` | Build 41 multistage build names.                              |
| `Location_Generic`  | `<map filename>`    | (none)             | Map title and description (see below).                        |

## JSON format

```json
{
  "UI_mainscreen_continue": "CONTINUE",
  "UI_mainscreen_solo": "SOLO"
}
```

Use the `$schema` field for VS Code validation:

```json
{
  "$schema": "https://raw.githubusercontent.com/pz-wiki-modding/pz-translation-data/refs/heads/main/PZ_Translation_Schemas/UI.schema.json",
  "UI_mainscreen_continue": "CONTINUE"
}
```

Replace `UI` in the schema URL with the translation type name from the table above.

## Map translations

Map names and descriptions use the `Location_Generic` type. The file must be named to match the map's `map.info` file and placed alongside other translation files:

```
media/
├── maps/MyMap/map.info
└── lua/shared/Translate/EN/
    └── MyMap.json
```

```json
{
  "$schema": "https://raw.githubusercontent.com/pz-wiki-modding/pz-translation-data/refs/heads/main/PZ_Translation_Schemas/Location_Generic.schema.json",
  "title": "My Map Name",
  "description": "A description shown on the spawn screen."
}
```

## Language codes

All languages supported by the game and their codes:

| Code | Language           | Code   | Language             |
| ---- | ------------------ | ------ | -------------------- |
| `EN` | English            | `FR`   | French               |
| `DE` | German             | `ES`   | Spanish (ES)         |
| `AR` | Spanish (AR)       | `PTBR` | Brazilian Portuguese |
| `PT` | Portuguese         | `RU`   | Russian              |
| `UA` | Ukrainian          | `PL`   | Polish               |
| `CN` | Simplified Chinese | `CH`   | Traditional Chinese  |
| `JP` | Japanese           | `KO`   | Korean               |
| `IT` | Italian            | `NL`   | Dutch                |
| `TR` | Turkish            | `CS`   | Czech                |
| `HU` | Hungarian          | `RO`   | Romanian             |
| `FI` | Finnish            | `DA`   | Danish               |
| `NO` | Norwegian          | `TH`   | Thai                 |
| `ID` | Indonesian         | `CA`   | Catalan              |
| `PH` | Filipino           |        |                      |

## Adding a new language

1. Create `media/lua/shared/Translate/<LanguageCode>/`.
2. Add a `language.txt` file describing the language.
3. Add font files if the language requires a non-Latin script.
4. Add translation `.json` files for each type you want to cover.

## VS Code schema setup

To get validation for all translation files, add the schema repository as a trusted domain in `.vscode/settings.json`:

```json
{
  "json.schemaDownload.trustedDomains": {
    "https://raw.githubusercontent.com/pz-wiki-modding/pz-translation-data": true
  },
  "json.schemas": [
    {
      "fileMatch": ["**/media/lua/shared/Translate/*/UI.json"],
      "url": "https://raw.githubusercontent.com/pz-wiki-modding/pz-translation-data/refs/heads/main/PZ_Translation_Schemas/UI.schema.json"
    }
  ]
}
```

A complete `settings.json` covering all types is available in the [PZ Translation Data releases](https://github.com/pz-wiki-modding/pz-translation-data/releases).
