# Startup Parameters

- **Wiki**: https://pzwiki.net/wiki/Startup_parameters (Build 42.17.0)

## How to set parameters

### Steam

Right-click the game → Properties → General → Launch Options. Enter parameters in the field, e.g. `-debug`.

### Shortcut

Navigate to the game folder (Steam Library → Manage → Browse local files), create a shortcut of `ProjectZomboid64.exe`, and add arguments to the Target field:

```
"C:\ProjectZomboid64.exe" -Xmx8192m -Xms8192m -- -debug
```

### Dedicated server (`StartServer64.bat`)

Add JVM arguments after the `-Xmx` line; add game arguments after `%1 %2` near the end. The `--` separator is not needed in the bat file.

## JVM argument ordering

JVM arguments must come first and be followed by `--` even when there are no game arguments:

```
-Xmx8192m -Xms8192m -- -debug
```

## Game arguments

### Client and server

| Argument                          | Description                                                 |
| --------------------------------- | ----------------------------------------------------------- |
| `-cachedir="<path>"`              | Sets the absolute path for the game's cache directory.      |
| `-nosteam`                        | Disables Steam API integration (same as `-Dzomboid.steam`). |
| `-console_dot_txt_size_kb=<size>` | Sets the maximum `console.txt` file size in kilobytes.      |

### Client only

| Argument                | Description                                                                                                                                                             |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-debug`                | Launches the game in debug mode.                                                                                                                                        |
| `-debuglog=<types>`     | Enables specific console log filters. Comma-separated list of `DebugType` values; prefix with `-` to disable. Examples: `-debuglog=All`, `-debuglog=Network,-Sound`.    |
| `-debugtranslation`     | Enables translation debug mode. Writes issues to `cachedir/translationProblems.txt` and allows reloading translation files with F12 in-game.                            |
| `-modfolders <folders>` | Controls which mod folders load and their order. Possible values: `workshop`, `steam`, `mods`. Omit a folder to disable it. Example: `-modfolders workshop,steam,mods`. |
| `-safemode`             | Reduced resolution, texture compression, 1x scales, disables WeatherShader and FBO. Use when the game fails to create a framebuffer.                                    |
| `-nosound`              | Disables game audio; also disables some voice chat aspects.                                                                                                             |
| `-novoip`               | Disables the VoiceManager (in-game voice chat).                                                                                                                         |
| `+connect <ip>:<port>`  | Directly connects to a server, bypassing the browser.                                                                                                                   |
| `+password <password>`  | Provides the server password when connecting.                                                                                                                           |
| `-imgui`                | Launches in debug mode with ImGui enabled.                                                                                                                              |
| `-imguidebugviewports`  | Launches in debug mode with ImGui in a separate window.                                                                                                                 |

### Server only

| Argument                 | Description                                                                                      |
| ------------------------ | ------------------------------------------------------------------------------------------------ |
| `-servername <name>`     | Sets the internal server name, which determines which save files are loaded/saved.               |
| `-adminpassword <pass>`  | Sets the default admin password, bypassing the prompt on first launch.                           |
| `-adminusername <name>`  | Sets a custom username for the default admin (does not remove existing admins).                  |
| `-port <port>`           | Overrides `DefaultPort` in the server INI.                                                       |
| `-udpport <port>`        | Overrides `UDPPort` in the server INI.                                                           |
| `-ip <ip>`               | Forces the server to bind to a specific IP address.                                              |
| `-steamvac <true/false>` | Enables or disables Valve Anti-Cheat.                                                            |
| `-disablelog=<types>`    | Disables specific console log filters.                                                           |
| `-debuglog=<types>`      | Enables specific console log filters.                                                            |
| `-statistic <seconds>`   | Enables multiplayer statistics monitoring at the given interval. Saved in `cachedir/Statistic/`. |
| `-coop`                  | Runs a coop server instead of a dedicated server.                                                |

## JVM arguments

### Client and server

| Argument                               | Description                                                                                               |
| -------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| `-Xms<size>m/g`                        | Minimum JVM heap allocation. Game will not start if the system cannot provide this. Example: `-Xms8192m`. |
| `-Xmx<size>m/g`                        | Maximum JVM heap allocation. Setting above physical RAM uses virtual memory. Example: `-Xmx8192m`.        |
| `-XX:+AlwaysPreTouch`                  | Touches every heap page at startup. Recommended when using ZGC (Java 21+).                                |
| `-Dzomboid.steam=<0/1>`                | Disables (`0`) Steam integration. Same effect as `-nosteam`.                                              |
| `-Dzomboid.ConsoleDotTxtSizeKB=<size>` | Sets the maximum `console.txt` size in kilobytes.                                                         |
| `-Ddeployment.user.cachedir="<path>"`  | Sets the cache directory (Linux only). Same as `-cachedir`.                                               |
| `-Ddebug`                              | Launches in debug mode (JVM-style equivalent of `-debug`).                                                |

### Client only

| Argument                              | Description                                         |
| ------------------------------------- | --------------------------------------------------- |
| `-Dargs.server.connect="<ip>:<port>"` | Connects directly to a server (same as `+connect`). |
| `-Dargs.server.password="<pass>"`     | Provides the server password (same as `+password`). |

## Launcher arguments (client only)

| Argument                | Description                                                    |
| ----------------------- | -------------------------------------------------------------- |
| `-pzexeconfig <config>` | Overrides the default launcher config `ProjectZomboid64.json`. |
| `-pzexelog <logfile>`   | Redirects launcher logging output to a file.                   |
