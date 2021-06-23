# Technical Explanation of "Deep Down" Graphics

* "Deep Down"
  * https://www.youtube.com/watch?v=jIRUImPWaOo



## Agenda

* Material
* Light
* Hair



## Material

![](images/2021_06_22_technical_explanation_of_deep_down/material-editor.png)


### Panta Rhei's objective

* High quality texture
  * Make it look richer than ever
* Unification of quality
  * Have similar quality no matter who makes it
  * Use it anywhere = separate lighting
* "Physically Based Rendering" to achieve


### Material

* Normal shading
  * OrenNayer + GGX
    * At the time of TGS exhibition, it was Oren Nayer + Blinn Phong, but it will be changed later
* Skin shading
  * Pre-integrated Skin Shading + GGX


### GGX

* Beautiful highlights of details
  * Leather, metal and plastic textures are more flexible than Blinn-Phong

![](images/2021_06_22_technical_explanation_of_deep_down/ggx.png)


### Skin sample

![](images/2021_06_22_technical_explanation_of_deep_down/skin-sample.png)


### Texture adjustment value

Value to adjust   | Meaning 
----------------- | -----------------------------------------------------------------
Albedo            | Diffuse reflectance Non-metals have color, metals are black
Reflectance       | Gloss reflectance Non-metals are almost black, metals have colors
Glossiness        | Surface smoothness
Normal            | Normal
Emissive          | Optional self-luminous component

Panta Rhei uses [3D-Coat][1] and MAYA shaders to make adjustments using these values


### Shader editor

* Create shaders with graphical editing
  * Graph method for editing by linking nodes

![](images/2021_06_22_technical_explanation_of_deep_down/shader-editor-1.png)

* Instant preview available

![](images/2021_06_22_technical_explanation_of_deep_down/shader-editor-2.png)


### Detail layer

![](images/2021_06_22_technical_explanation_of_deep_down/detail-layer-1.png)

![](images/2021_06_22_technical_explanation_of_deep_down/detail-layer-2.png)


### Water monster

![](images/2021_06_22_technical_explanation_of_deep_down/water-monster.png)


### Screen space effect 

![](images/2021_06_22_technical_explanation_of_deep_down/screen-space-effect.png)


### Shader editor pros/cons

* Pros
  * Artists can create while checking the appearance for themselves
* Cons
  * The number of nodes increases and tends to be complicated
  * Performance is not considered


### Comparison with the conventional

* Conventional (MT FRAMEWORK)
  * Material (setting based on empirical rules)
  * A combination of shader nodes
* Currently (Panta Rhei)
  * Material (Physically based. Created with a small number of parameters)
  * Artist-based shaders



## Light

### Panta Rhei's objective

* Dynamic light source processing as much as possible
  * Automatically generated dungeons
    * No lightmap in the dungeon
  * handle indirect lighting dynamically
* Improved rendering quality


### Lighting

* HDR lighting & linear space
  * Rendered in FP16
* In-game lighting
  * Direct lighting
    * Tile-based deferred + forward
  * Indirect lighting
    * Irradiance volume + parallax correction environment map


### Rendering Pipeline

![](images/2021_06_22_technical_explanation_of_deep_down/rendering-pipeline-1.png)


### GBuffer

* Albedo and Reflectance compressed with YCbCr
  * Cb and Cr are stored alternately in pixel units in RT3
* Options: Save decal information, BRDF type

![](images/2021_06_22_technical_explanation_of_deep_down/gbuffer.png)


### Problems with YCbC

* Albedo and reflectance with YCbCr
  * Due to the nature of encoding, color difference information
  * Colors close to black cannot be reproduced accurately
    * Purple etc. occur when exposed to high brightness
    * 8-bit accuracy
* It is better to put RGB normally
  * Consider introducing Metallic's idea for the same capacity


### Rendering Pipeline

![](images/2021_06_22_technical_explanation_of_deep_down/rendering-pipeline-2.png)


### Tile Based Deferred Shading

* Two-stage configuration
  * Create light list
    * Creating light information that is also used for translucency, etc.
* Direct lighting processing


### Light list

* Create a light list by dividing the image into 16x16 units
  * Create a frustum from tile depth information
    * Divide the frastam into 32 pieces in the line-of-sight direction
      * The 32nd is a frustum in the range of maximum and minimum depth values
      * The rest is a frustum divided evenly from the minimum value
      * Judgment of intersection with the light source for each frustum

![](images/2021_06_22_technical_explanation_of_deep_down/light-list.png)


### Creating a light list

* Calculate and save the number of lights after culling
  * Get memory blocks from the writelist pool
    * Keep head offset and number of tiles
* The pool is uint2's Structured Buffer
  * 32-bit write index
  * 32-bit culling mask


### Light culling

![](images/2021_06_22_technical_explanation_of_deep_down/light-culling.png)


### Direct lighting

* Processed as a light source with size
  * Spherical light source only
    * Light intensity, magnitude, distance squared attenuation
  * Points, spots, directional
  * Supports shadow and projected textures


### Example of direct lighting

![](images/2021_06_22_technical_explanation_of_deep_down/direct-lighting-example.png)


### Direct lighting shader

* Process images in 16x16 units with CS
  * Get from light list
    * Get only lights with the 32nd bit enabled
    * Stored in Shared Memory for each light type
  * Expand loops by light type
    * Use the best calculation within each loop
    * VGPR bloated when shadow filter is involved


### Rendering Pipeline

![](images/2021_06_22_technical_explanation_of_deep_down/rendering-pipeline-3.png)


### Forward

* Used for skin and translucent
  * Lighting using the results of the light list
* Search for the mask of the light list from the Z value at the time of drawing
  * Supports shadow and texture projection


### Rendering Pipeline

![](images/2021_06_22_technical_explanation_of_deep_down/rendering-pipeline-4.png)


### Indirect lighting

![](images/2021_06_22_technical_explanation_of_deep_down/indirect-lighting.png)

Processed at once with pixel shader

* Irradiance Volume
* Screen space reflection
* Parallax Corrected Cubemap

![](images/2021_06_22_technical_explanation_of_deep_down/indirect-lighting-methods.png)



## Irradiance volume


### Direct lighting

![](images/2021_06_22_technical_explanation_of_deep_down/irradiance-volume-1.png)


### Direct lighting + irradiance volume

![](images/2021_06_22_technical_explanation_of_deep_down/irradiance-volume-2.png)


### Irradiance volume

* Diffuse reflection of indirect lighting
* The light that hits the surface is reflected
  * You can also place fake and invisible luminescent polygons

![](images/2021_06_22_technical_explanation_of_deep_down/irradiance-volume-3.png)

* Created while the game is loading
  * Use voxel cone tracing
* Create voxels for your scene
  * Output voxel information from PS to list for each model
    * Illuminated color information and occlusion information
  * Enter the list in voxels (256x256x256)
  * Create voxel mipmaps
* Creating the final irradiance volume
  * 128x128x128 structure that covers the entire scene
  * Cone tracing by dividing the entire spherical surface into 12 directions
    * Each cone trace result is added as 4 base colors
      * Implemented based on Farcry 3
    * Stored in 3 128x128x128 3D textures



## Screen space reflection

* Image-based ray tracing

![](images/2021_06_22_technical_explanation_of_deep_down/screen-space-effect-2.png)

* Linear exploration and dichotomy
  * Glossy reflection is not supported



## Parallax correction environment map

* also called "Parallax Corrected Cubemaps"


### Direct lighting

![](images/2021_06_22_technical_explanation_of_deep_down/irradiance-volume-1.png)


### Direct lighting + Parallax correction environment map

![](images/2021_06_22_technical_explanation_of_deep_down/parallax-correction-environment-map-1.png)


### Parallax correction environment map

* Used for diffuse and specular reflections
* Essential for metal texture

![](images/2021_06_22_technical_explanation_of_deep_down/parallax-correction-environment-map-2.png)

* Up to 96 in one scene with "deep down"
  * Surface roughness changes the mip level of the environment map
    * Resolution up to 128x128, mipmap up to 2x2
    * Color information is retained in RGBE format on R8G8B8A8

![](images/2021_06_22_technical_explanation_of_deep_down/parallax-correction-environment-map-3.png)

* Environmental map capture and filtering on load
  * Taken with a tetrahedron(四面体)
  * Create all mip levels at once with CS
* Parallax correction environment map exploration with stackless tree
  * AABB only BVH
  * Works well as it has almost the same branching direction


## Indirect lighting

* 效果对比


### Direct lighting

![](images/2021_06_22_technical_explanation_of_deep_down/irradiance-volume-1.png)


### Indirect lighting

![](images/2021_06_22_technical_explanation_of_deep_down/indirect-lighting-2.png)



[1]:https://3dcoat.com/
