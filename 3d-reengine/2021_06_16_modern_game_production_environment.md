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

#### Demo 1 - Easy stage creation

[![RE ENGINE - Easy Stage Creation](https://res.cloudinary.com/marcomontalbano/image/upload/v1624010524/video_to_markdown/images/youtube--_v4uhulozwg-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://www.youtube.com/watch?v=_v4uhulozwg "RE ENGINE - Easy Stage Creation")

#### Demo 2 - Prefab and multi-scene

#### Demo 3 - Check commit history and differences



## Part II - Masaaki Korematsu


## Postscript

* upload to youtube and use `video-to-markdown`
  * https://github.com/marcomontalbano/video-to-markdown
  * https://video-to-markdown.netlify.app/
