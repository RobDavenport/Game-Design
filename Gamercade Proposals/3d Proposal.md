## Summary
A suggestion for a simple 3D functionality extension for Gamercade. 
## Motivation
At its core, Gamercade will remain a Neo-Retro Fantasy Console. We define Neo-Retro as:

>  In our definition, it means something which is retro-looking, but still has many capabilities of modern times. In the case of Gamercade, this means that the games themselves are retro-like in their appearance, such as with lower resolutions and pixel graphics.

Since Gamercade targets the mid '90s to early 2000s home game consoles, it would be realistic to also support a basic form of 3D. Consoles of that generation include the Sega Saturn, PlayStation, and Nintendo 64. These consoles featured specific hardware to accelerate 3D graphics compared to the software implementations of the previous generation. As Gamercade is a Neo-Retro fantasy console, it should match the capabilities of those consoles. Those consoles are severely outclassed by modern hardware, but they still created engaging and memorable experiences.

Of course, the complexity jump from 2D to 3D is large, but the added dimension opens up substantially more options for modern day creatives. As Gamercade currently only natively supports 2D, developers are somewhat restricted in the kinds of experiences they can create. Adding the third dimension opens up the console to showcase the talent of 3D artists like modelers, riggers, and animators. The recent popularity of low poly artwork and stylized games show that there is still large appeal for graphically simplistic games. This is further enhanced by the faster workflow and simpler development process of these art styles. 

### Example References:

[Compound VR](https://www.youtube.com/watch?v=x0N56nCqLu4) - 3D FPS VR game with pixel art textures, no lighting.

[Sketchfab #256fes Models](https://sketchfab.com/search?q=256fes&type=models) - Models from the 256fes hashtag on twitter. 256 triangle limit with 256x256 textures.

[BlockBench](https://www.blockbench.net/) - Low Poly 3D modeling software.

[Crocotile3D](https://crocotile3D.com/) - 3D Modeling software for making models and environments with tiled textures.
Any Nintendo 64 or PlayStation game.

[RPG Paper Maker](https://rpg-paper-maker.com/) - Unique 3D engine. Uses 2D sprites for a very 2.5D feel.
## Goals & Targets
### Primary Goals
The primary goals focus on simplicity, approachability, and usability
#### Basic 3D Rendering
We should provide a fixed-function rendering pipeline with a few adjustable knobs. Removal of user-defined shaders drastically eases the introduction into 3D. Use easy to understand shading models like vertex-colors, basic texture maps, flat shading, or Blinn-Phong shading. Lighting should be limited to a few basic light types, and the number of lights should also be limited.
#### Simple 3D API Extension
Similar to the 2D API via `GraphicsParameters` and `sprite` functions, simple one-liners or a small number of function calls to draw 3D objects. Users should not need to worry about completely understanding the graphics pipeline, cameras, or GPU memory layout, and should instead be provided with a solid set of defaults to be tweaked when necessary.
#### Editor Improvements & Accurate Viewer
The editor should represent exactly what the models would look like in-game. This means adjustments to the editor to support 3D rendering of models with animations, file importers, and bundler updates. Using a WYSIWYG workflow (similar to the Sound Engine) ensures an accurate, simple, and easily understandable representation of assets.
### Non-Goals
#### Photorealism
Realistic shaders and high definition graphics are not necessary. They would severely increase performance requirements, difficulty in asset creation, and go against the Neo-Retro focus of the console. Additionally, the texture sizes needed to support this would take up much of the ROM's available capacity.
#### Scene/Level Editor
Gamercade is not intended to be a game engine. The concept of a scene or level is up to the decisions of the developer. Developers wanting to make use of other (or their own) tools to assist in Gamercade development can make use of the `datapack` feature set.
#### Ultra High Performance
While we are not specifically aiming for bad performance, this 3D proposal just covers basic functionality and "good enough" performance. Due to the constraints on ROM size, its likely that most modern GPUs and iGPUs will be able to handle whatever we can throw at it without much effort.
#### Full Rendering Pipeline Access
Due to the complexity of the full rendering pipeline, we should not expose this to users. For example, wanting to support custom vertex shaders or fragment shaders means we need to consider data layout on the GPU itself, or some kind of reflection/parsing of the shader code to ensure provided parameters get to where they need to.
## Implementation Details
### Change Summary
- Adjustments to the ROM to include optional 3D Assets
	- Support Models and Textures
	- "Global Rendering Settings"
- Creation of Gamercade 3D Library
	- 3D Rendering of Meshes
	- Skeletal Animation
	- Model, Texture, Animation loading capabilities
	- Import functionality for 3D assets
- Add required API functions to:
	- Console runtime
	- gamercade_rs
	- API Docs
### Graphics Engine Specs & Limitations
The following limitations are built with the core principals in mind: they should be simple yet flexible, and support the Neo-Retro goal of the project. Some of these features are influenced by modern day standards (such as tangent space normal maps, and 4-bone influence animation), other constraints will keep things back within the retro feel.

- General
	- Right Handed Coordinate System
	- CCW Winding Order
	- Automatic Backface Culling
	- Column Major Matrices
	- Limited Polygon Budget (See Table Below)
- Textures
	- 32bit RGBA
	- 512x512 max size (~1mb)
	- Nearest Filtering Only
	- Repeating/Tiling Only
	- Up to u16 Texture Slots (65,535)
- Meshes
	- Only indexed and triangulated geometry
	- Supported Vertex Parameters
		- Position
		- UV
		- Color
		- Normal
		- Tangent
	- Up to 4 Textures per Mesh
		- Diffuse Map (Albedo)
		- Normal Map (Tangent Space)
		- Specular Map 
		- Emissive Map
- Camera
	- Orthographic or Perspective Camera
		- Adjust Near & Far Planes
		- Adjust FOV
- Lighting 
	- Up to 8 of:
		- Ambient Lights
		- Directional Lights
		- Point Lights
		- Spotlights
	- Lights can be adjusted freely at runtime
	- Fixed Function Rendering, based on Blinn-Phong, enable via flags
		- Toggle diffuse map or base color
		- Toggle specular map or specular material properties
		- Toggle fragment normal via normal map, vertex parameters, or both
		- Toggle emissive material
- Skeletal Animation
	- Keyframe Driven Animation
	- No Interpolation
	- Support for Location, Rotation, and Scale
	- Up to 4 Bone Influences per Vertex
	- Up to 64/128 bones per skeleton
#### Rendering Limits & Gas
In order to simulate the limited GPU of the console, a limited amount of **Gas** is available to be used for each frame. Each draw call will consume gas, depending on the complexity of the function. The amount of gas available depends on the target frame rate of the rom, as well as the rendering resolution. Therefore, high resolutions and frame rates will provide less gas per frame, and low resolutions and frame rates will provide more gas per frame. Gas will be replenished up to the maximum value on each refresh. This creates opportunity for developers to optimize their projects by tweaking graphics settings (resolution, fps) or art asset complexity to match their desired vision

The base gas amount is `512,000,000` for the medium resolution. A **scaling factor** can be calculated for other resolutions as follows:
`scaling_factor = sqrt(medium res total pixels / other res total pixels)`
Below is a list of the gas available for each of the 16:9 resolutions:

| Resolution | Total Pixels | Scale Factor | Total Gas Per Second |
| ---------- | ------------ | ------------ | -------------------- |
| UltraLow   | 9,216        | 3.75         | 1,920,000,000        |
| VeryLow    | 14,400       | 3            | 1,536,000,000        |
| Low        | 57,600       | 1.5          | 768,000,000          |
| **Medium**     | **129,600**      | **1**            | **512,000,000**          |
| High       | 230,400      | 0.75         | 384,000,000          |
| VeryHigh   | 921,600      | 0.375        | 192,000,000          |
| UltraHigh  | 2,073,600    | 0.25         | 128,000,000          |

As the available gas is listed in gas per second, the amount available per frame is simply taking the per-second amount and diving it by the ROM's `fps` value. See the following table for some examples:

| Resolution | Gas Per Second | FPS | Gas Per Frame |
| ---------- | -------------- | --- | ------------- |
| Medium     | 512,000,000    | 30  | ~17,066,666   |
| Medium     | 512,000,000    | 60  | ~8,533,333    |
| High       | 384,000,000    | 30  | 12,800,000    |
| High       | 384,000,000    | 120 | 3,200,000     |
| Low        | 768,000,000    | 60  | 12,800,000    |
| UltraHigh  | 128,000,000    | 24  | ~5,333,333    |

Each rendering function call will consume the gas value according to the following table:

| Base Parts           | Cost | Note                                              |
| -------------------- | ---- | ------------------------------------------------- |
| Polygon Base         | 300  | Base cost for every triangle                      |
| Texture Lookup       | 100  | Multiplied for each texture lookup                |
| Param: UV            | 100  | Base cost to use UVs for textures                 |
| Param: Vertex Color  | 150  | Base cost to use Vertex Colors                    |
| Param: Normal        | 150  | Base cost to use Normals for lighting             |
| Param: Tangent       | 150  | Base cost to use Tangents for normal maps         |
| Lighting Calculation | 100  | Multiplied for each lighting calculation          |
| Skeleton (Per Bone)  | 10   | Multiplied for each bone, once per draw call      |
| Skinning (Weights)   | 30   | Multiplied for each triangle for each max weights |

This allows us to build a couple of combination piece depending on the complexity of the draw call:

| Combinations       | Cost | Note                                                   |
| ------------------ | ---- | ------------------------------------------------------ |
| Add Diffuse Map    | 200  | Include UV + Texture Lookup                            |
| Add Emissive Map   | 100  | Texture Lookup                                         |
| Add Specular Map   | 200  | Texture Lookup + Lighting Calculation                  |
| Add Normal Map     | 350  | Texture Lookup + Incude Tangent + Lighting Calculation |
| Add Basic Lighting | 250  | Include Normal + Lighting Calculation                  |

// TODO: Add more examples & Simulations
### API Proposal
For simplicity sake, a **stateful API** is suggested for the current implementation. This is due to the WASM Module <--> Host Communication layer only supporting (i32, i64, f32, f64) datatypes, and managing pointers between these can be handled at a later date. The following draft is still a work-in-progress

```rust
// Model Matrix Manipulation
set_model_translation(x: f32, y: f32, z: f32);
set_model_rotation(x: f32, y: f32, z: f32, w: f32);
set_model_scale(x: f32, y: f32, z: f32);
clear_model_transform();
set_model_transform(transform_ptr: i32); // Column-Major

// Texture Settings (Index is a 16bit number)
set_texture_diffuse(index: i32);
set_texture_normal(index: i32);
set_texture_specular(index: i32);
set_texture_emissive(index: i32);
set_textures_multi(textures: i64); // Bitpacked values

// Camera Manipulation
camera_translate_local(x: f32, y: f32, z: f32);
camera_translate_global(x: f32, y: f32, z: f32);
camera_set_location(x: f32, y: f32, z: f32);
camera_look_to(x: f32, y: f32, z: f32); // Additional "Look At" variant
camera_rotate_around_local_x(radians: f32); // Also Y and Z Variants
camera_rotate_around_global_x(radians: f32); // Also Y and Z Variants

// Light Settings
set_light_direction(slot: i32, x: f32, y: f32, z: f32); // Affects: Directional, Spot
set_light_range(slot: i32, value: f32); // Affects: Point, Spot
set_light_location(slot: i32, x: f32, y: f32, z: f32); // Affects: Point, Spot
set_light_color(slot: i32, r: f32, g: f32, b: f32);
set_light_enabled(slot: i32, value: i32); // 0: Disabled, Non-0: Enabled

// Draw Calls
// TODO
draw_static_mesh(mesh_id: i32); // Uses the previously set transformation

// Rendering Flags
// TODO

// Animation
// TODO
draw_animated_mesh(mesh_id: i32, animation_id: i32, time: f32) // Mesh & Skin are predefined

```
## Potential Issues and Mitigation
### Size of 3D Assets vs ROM Size Limits
The inclusion of 3D models and their accompanying data (textures, vertex colors, animation and skinning data) will heavily increase storage needs within the ROM. We should place some limits on the assets themselves, such as maximum vertex counts, triangle counts, or texture sizes.
### 3D Rendering and Lighting
It's quite rare to see unlit 3D games, and this is likely the reason for such a severe complexity jump from 2D -> 3D. We should limit the number and kinds of lights that the engine supports to keep things simple and straightforward. Forcing lights to be managed via code also ensures they are used for improving gameplay and not purely cosmetic.
## Possible Long Term Additions
The following contains a list of possible extensions that could be made after the initial MVP for 3D is released. These are sorted in generally by importance, although not a strict guideline by any means.
### Built-in Primitive Drawing
Similar to the already existing 2D primitives like `circle` `rect` and `line`, it would be beneficial to have simple built-in primitives to use for prototyping or debugging during development. These primitives should be flexible and game-ready, such as having proper vertex UVs, tweakable vertex colors, etc. This may relate to [[#3D Data Access in Code]] since we may also be editing the geometry at runtime.
A good list of shapes to start out with are:
```js
cube(width, height, depth)
plane(width, height)
sphere(radius, segments)
circle(radius, segments)
cylinder(radius, segments)
cone(radius, height, segments)
```

Width, height, depth, radius are self explanatory. Segments in this case refers to the polycount or quality of the shapes. For example, a sphere with low segments would look very blocky, and therefore a sphere with high segments would look very smooth and rounded. 
### Additional Stateless API
A stateless API could also be implemented, but would require passing of pointers to values rather than the stateful approach. All the parameters needed to draw a mesh would be included as parameters to the function call, instead of relying on in-between state. Example:

```rust
// Updating the model transform matrix:
// Stateful Approach
set_model_translation(x: f32, y: f32, z: f32);
set_model_rotation(x: f32, y: f32, z: f32, w: f32);
set_model_scale(x: f32, y: f32, z: f32);
draw_mesh(mesh_parameters: i32);

// Stateless Approach
set_model_transform(transform_ptr: i32);
draw_mesh(mesh_parameters:i32);

// Alternative Stateless:
draw_mesh(mesh_parameters: i32, transform_ptr: i32);
```
### 3D Data Access in Code
Allow accessing the 3D data used for rendering, such as uploading meshes or textures to the GPU. This opens up the option of procedurally generated content, such as terrain, or even procedural meshes and textures to be used for rendering later. Additionally, this could allow for things like IK bones driven by code, or procedural animation.
### Graphics Engine Tweaking
Allow users to adjust simple settings like:
- Texture filtering, mirroring, or repeating behavior
- Animation system adjustments, tweak the max bone influences per vertex from 1-4
- Winding order of triangles, backface culling
### Animation Interpolation
Original implementation is completely keyframe driven animation. This feature would allow interpolation between keyframes to smooth transitions between. Some research is necessary to provide the requirements and limitations of this system, such as the kinds of interpolation (or only supporting those built-in to GLTF), blending, etc.
### Animation Blending
Support blending for animations between 2 (or N) number of states. If [[#3D Data Access in Code]] is done, it may be possible to omit this entirely and leave it up to the user to handle themselves.
### Support for Multiple Viewports or Cameras
Add functionality to render multiple scenes from different views, for example: split screen multiplayer.
### Custom Shaders
Expose shader access to developers to allow them to write their own shaders for custom effects.
### Performance Oriented API Additions
Mesh instancing, automated batching, etc, for advanced users who want to push the limit.
