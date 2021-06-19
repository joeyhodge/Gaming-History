# RE ENGINE - Modern Game Production Environment

* by Yusuke Ichiyama and Masaaki Korematsu



## Preface

### Why a new production environment?

* Further scale-up of development
  * Transition to 8th generation
  * Increased memory and computing resources
  * In the production environment of MT FRAMEWORK, there is a limit to efficiency.
* I had to make a big review of the production environment


### Agenda

* Yusuke Ichiyama
  * Editor introduction and internal design
  * Asset design premised on co-editing
* Masaaki Korematsu
  * Telemetry
  * Stable environment distribution
  * C# script and C++ reflection



## Part I - Yusuke Ichiyama

* Editor introduction and internal design
  * Simple demonstration
  * Relationship between editor and runtime
  * Cooperation between editors
  * Undo support
* Asset design premised on co-editing
  * Common things in scene asset editing
  * Can assets be merged?
  * What's important for merging assets
  * What I learned from operation


### Introduction to Editor

#### Engine Editors

* Studio
  * Generic term for editors
    * Integrated game development environment consisting of about 40 types of editors
      * I can't make it by myself
      * Requires simple rules and frameworks
* Runtime
  * Game runtime
    * Write the engine code in C++ and the game code in C#

#### Studio language and external libraries

* WPF (.NET4.5) + C#
  * Rich user interface
  * Separation of display and function by Binding
* Livet
  * Leverage MVVM infrastructure, update notifications, event listeners
  * https://visualstudiogallery.msdn.microsoft.com/ee198b62-fa45-496c-9c1c-e8f63b43f677
  * https://github.com/runceel/Livet
* Avalon Dock
  * WPF docking window library
  * https://avalondock.codeplex.com/
  * https://github.com/Dirkster99/AvalonDock

#### Demo - Easy stage creation

[![RE ENGINE - Easy Stage Creation](https://res.cloudinary.com/marcomontalbano/image/upload/v1624010524/video_to_markdown/images/youtube--_v4uhulozwg-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://www.youtube.com/watch?v=_v4uhulozwg "RE ENGINE - Easy Stage Creation")

#### Demo - Prefab and multi-scene

[![RE ENGINE - Pretab](https://res.cloudinary.com/marcomontalbano/image/upload/v1624014584/video_to_markdown/images/youtube--Kh2W33I1uiY-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://youtu.be/Kh2W33I1uiY "RE ENGINE - Pretab")

#### Demo - Check commit history and differences

[![RE ENGINE - Check commit history and differences](https://res.cloudinary.com/marcomontalbano/image/upload/v1624014691/video_to_markdown/images/youtube--vbOfPkKumMA-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://youtu.be/vbOfPkKumMA "RE ENGINE - Check commit history and differences")

#### Scene editor

* Create a hierarchy with GameObject and Component
* Variations with prefabs
  * Inheritance, use of prefabs in prefabs
* Multi-scene
  * Compose one scene with multiple scenes
  * Loading and unloading by scene

#### Asset browser

* Asset manipulation front end
  * Asset management
    * File update detection (monitoring with FileSystemWatcher)
* Version control
  * Updates, commits, history, etc.
  * Use of Perforce function
* Understanding dependencies
  * Asset reference, non-reference
    * Relationships stored in metadata


### Relationship with game runtime

#### Relationship between Studio and Runtime

* Works in a separate process
  * Studio is unaffected by Runtime crashes
  * Studio and Runtime are one-to-one
    * Runtime can create multiple views
    * Resources can be shared
  * Studio receives type information from Runtime

```
+--------+            +---------+
|        | - - - - -> |         |
| Studio |   TCP/IP   | Runtime |
|        | <- - - - - |         |
+--------+            +---------+
```

#### Host the runtime display

* Fit the program of another process into the docking frame of [AvalonDock][1]

![](images/2021_06_16_modern_game_production_environment/host-runtime-display-1.png)

* Use WindowsFormsHost(HwndHost)
  * Create WindowHandle for parent and child
    * Studio can handle multiple Runtime drawing results
  * Click events etc. are processed on the runtime side
    * Feedback to the editor via TCP/IP
* In AvalonDock, the docking window is created by UserControl.
* In WPF, user controls don't have window handles, so you can explicitly create window handles.
You need this method.
* Since the runtime display is a separate process, the windows are parented and childed.
  * If you look at it with the Spy tool, you can see that the window handles are parent-child. 

![](images/2021_06_16_modern_game_production_environment/host-runtime-display-2.png)

#### Studio editor is always master

* Clarify master-slave relationship
  * Runtime is a slave of the editor
    * Existence that displays drawing results
  * Editor has all data and editing rights
    * Do not create a structure where data is determined on the runtime side

#### Demo - Crash recovery

[![RE ENGINE - Crash Recovery](https://res.cloudinary.com/marcomontalbano/image/upload/v1624090989/video_to_markdown/images/youtube--B64_4QKgLu0-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://www.youtube.com/watch?v=B64_4QKgLu0 "RE ENGINE - Crash Recovery")

#### Crash recovery

* Data is protected in the event of a crash
  * In MT FRAMEWORK, the editor and data were blown away together
  * Separate process of runtime
  * Editor has all data and editing rights


### Cooperation between editors

#### Editors coexist in Studio

* About 40 types
  * Need a well-controlled creation policy and means
    * Greatly affects expandability and maintainability
* Editors work together
  * A screen that displays only the edit target and a screen that displays the entire scene
  * While watching the game screen

#### Cooperate around assets

* Editor edits assets
  * The editor display changes as the asset changes
  * Event driven with a focus on assets




## Part II - Masaaki Korematsu



## Postscript

* upload to youtube and use `video-to-markdown`
  * https://github.com/marcomontalbano/video-to-markdown
  * https://video-to-markdown.netlify.app/



[1]:https://github.com/Dirkster99/AvalonDock
