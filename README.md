# WorldGeneratorApi

[![Build Status](https://travis-ci.com/rutgerkok/WorldGeneratorApi.svg?branch=master)](https://travis-ci.com/rutgerkok/WorldGeneratorApi)

Designing your own world generator for Bukkit is hard. Bukkit doesn't let you hook into the Minecraft terrain generator, so if you just want to change the shape of the terrain, you would need to rewrite the entirety of the Minecraft terrain generator. Alternatively, you can hook into Minecraft internals, but this is tricky and tends to break on every Minecraft update.

WorldGeneratorApi provides a clean API to design your own world generator, while still using components of Minecraft if you want. In just nine lines of code, we can create a complete plugin that generates flat worlds:

```java
public class YourPlugin extends JavaPlugin {
    public ChunkGenerator getDefaultWorldGenerator(String worldName, String id) {
        return WorldGeneratorApi.getInstance(this, 0, 3).createCustomGenerator(WorldRef.ofName(worldName), generator -> {
            // Code modifying the world generator goes here
            generator.setBaseChunkGenerator(chunk -> {
                chunk.getBlocksForChunk().setRegion(0, 0, 0, 16, 70, 16, Material.STONE);
            });
        });
    }
}
```

![A decorated, flat world](https://rutgerkok.nl/afbeeldingen/minecraft/worldgeneratorapi.jpg)

As you can see, only the shape of the terrain is modified, the rest of the world generator is untouched. Want to disable flowers and grass? Add `generator.getWorldDecorator().withoutDefaultDecorations(DecorationType.VEGETAL_DECORATION);`. Don't like caves and ravines? Add `generator.getWorldDecorator().withoutDefaultDecorations(DecorationType.CARVING_AIR);`. 

## Features
* Control the base shape of the terrain.
* Disable vanilla resources (caves, flowers, villages, etc.)
* Add custom resources
* Supports the async chunk generator of Paper

## Tutorials/how to use
Server admins only need to download this plugin (see the [releases tab](https://github.com/rutgerkok/WorldGeneratorApi/releases)) and place it in their plugins folder, alongside with the plugin that asked you to download this API.

Are you a plugin developer? We have [a wiki](https://github.com/rutgerkok/WorldGeneratorApi/wiki) with serveral tutorials to get you started. The source code also contains lots of JavaDoc.

## Limitations
* There is no proper way to plug in your own biome generator.
* You always need to provide your own custom base chunk generator, it is not yet possible to use the vanilla ones.
* There is no way to add custom biomes yet.
* There is no way to spawn entities yet.

## Plugins using WorldGeneratorApi
* [DoughWorldGenerator](https://github.com/rutgerkok/Dough/) - a plugin that lets you modify the shape of your terrain. It supports all variables from the old Customized world type, plus some variables from the (now defunct) plugin TerrainControl.
* [PancakeWorldGenerator](https://github.com/rutgerkok/PancakeWorldGenerator/) - generates flat lands, but will all resources (ores, trees, structures, etc.) present. Useful for survival servers that want a flat world.

## License
License is [MIT](LICENSE), so you can freely use this API, even in premium plugins. Just put up a note that you're using WorldGeneratorApi.

## Compiling
We use Maven. Maven can be a bit tricky to install (you need to modify the environment variables of your computer), but once you have managed to do that, just run:

    mvn install

You'll end up with a file `./WorldGeneratorApi-1.0.jar` (version number will be different of course), which is a ready-to-use plugin. There's also the file `./worldgeneratorapi/target/worldgeneratorapi-1.0.jar`, which is just the API. This file can be used by plugin developers to code against: it will not run on the server, as it is missing the implementation.

## Bug reports, feature requests and pull requests
Contributions are always welcome! Just open an issue or pull request. 
