# FatEngine Architecture

## About FatEngine

 * a [CryENGINE][1] clone, with codebase cleanup
 * a learning project of myself
 * [CryENGINE technial notes][2]


## Architecture

```
+---------------------------------+ - - - - - - - - - - - - -
|           Lancher.exe           |       Game Starter
+---------------------------------+ - - - - - - - - - - - - -
|            Game.dll             |        Game Logic
+---------------------------------+ - - - - - - - - - - - - -
|                                 |
|      Engine Modules (*.dll)     |      Engine Modules
|                                 |
+----------------+----------------+ - - - - - - - - - - - - -
| FatCommon.lib  |                |
|----------------+                |   Engine Common Library      
|          FatPlatform.lib        |
+---------------------------------+ - - - - - - - - - - - - -
| Windows | OpenBSD | Linux | ... |     Operating System
+---------------------------------+ - - - - - - - - - - - - -
```

### Game Starter

 * start the game using IGameStartup

### Game Logic

 * all game-specific stuff here
 * game developer only need to implement this .dll
 * Implement IGame, IGameFramework, IGameStartup interface

### Engine Modules

 * FatSystem, implements all fundermental singleton managers
 * FatRenderer, DX9/DX11/OpenGL renderer
 * Fat3D, 3dengine stuff
 * ...

### Engine Common Library

 * FatCommon, engine common classes (String, Containers, SmartPtr)
 * FatPlatform, platform abstract layer


## Folder Layout

### /

 * for release, only copy [Bin64] & [Game] to user

```
[Bin32]          32-bit binary (.exe/.dll/.lib)
[Bin64]          64-bit binary
[BinTemp]        Intermediate files of building
[Code]           Engine & game code
[Game]           Game assets
```

### /Code

```
[Docs]           Documentation
[FatEngine]      Engine code
[Game]           Game/Launcher/UnitTest code
[SDKs]           External dependencies
[Solutions]      VS2017 & emake files
```


## Build System

 * Platform supports Windows/OpenBSD/Linux/Android
 * Using different C++ compilers make finding bugs easier

### VS2017

 * /Code/Solutions/VS2017
 * MSVC Compiler
 * Windows

### emake

 * [https://github.com/skywind3000/emake][3]
 * Debian(linux) - gcc
 * OpenBSD - clang
 * Android

[1]:https://github.com/CRYTEK/CRYENGINE
[2]:https://github.com/kasicass/blog/blob/master/fat3d/2020_08_08_cryengine_technical_notes.md
[3]:https://github.com/skywind3000/emake
