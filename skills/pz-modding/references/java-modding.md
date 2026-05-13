# Java Modding

- **Wiki**: https://pzwiki.net/wiki/Java (Build 42.18.0)

Java mods call game code directly and face almost no API limitations, but they cannot be loaded automatically from Workshop installs and must be manually placed by the user. Java class files are also tightly coupled to PZ's build — any change to the classes you modify will break your mod on the next game update.

Start with Lua if you can. Use Java modding only when Lua cannot reach what you need.

## Tools

Three tools exist that make Java modding significantly less painful:

- **[ZombieBuddy](https://pzwiki.net/wiki/ZombieBuddy)** (recommended) — a Java agent that injects code into any game method using `@Patch` annotations with `OnEnter`/`OnExit` hooks. Requires no full class rewrite.
- **[Leaf](https://pzwiki.net/wiki/Leaf)** — a mod loader with targeted modification helpers. Reduces how often you need to update your mod.
- **[Necroid](https://pzwiki.net/wiki/Necroid)** — compiles mods on the fly; as long as the API surface you use doesn't change, no update is needed.

## Java version

As of Build 42.13.0, PZ uses **Java 25**. You must compile with the same version:

```bash
javac -cp "GameFiles\projectzomboid.jar" "path/to/YourFile.java"
```

Download Java from [Oracle](https://www.oracle.com/java/technologies/downloads/) or use [OpenJDK](https://openjdk.org/).

## Decompiling game code

PZ ships compiled `.class` files inside `projectzomboid.jar`. Decompile it to read and understand the game's Java source before writing patches. Use a Java decompiler (Fernflower via IntelliJ IDEA, CFR, or Procyon). See the [Decompiling game code wiki page](https://pzwiki.net/wiki/Decompiling_game_code) for the step-by-step workflow.

The [JavaDocs](https://projectzomboid.com/modding/) provide the public API surface without decompiling, but decompiling is needed to understand private logic.

## Deploying class files

There is no `media/` subfolder for Java class files. To deploy:

1. Compile your `.java` into a `.class` file.
2. Place the `.class` file in the game folder mirroring the Java package path. For example, a class in `zombie.iso` goes in `GameFiles/zombie/iso/`.

The game will load class files from the `zombie/` directory even if it does not exist by default. To override a vanilla class, the directory and file name must exactly match the vanilla path.

Because class files live outside the mod's `Contents/` folder, users must install them manually — this is the main distribution friction for Java mods.

## Risks and update fragility

Any PZ update that changes a class you have replaced will break your mod. This is a fundamental constraint: you are replacing compiled bytecode, not hooking into a stable API. Tools like ZombieBuddy and Leaf reduce this by targeting specific method injection points rather than full class replacements, but the risk never fully disappears.

Lua mods almost never break on updates unless PZ explicitly removes or renames an API. Java mods can break on any refactor.
