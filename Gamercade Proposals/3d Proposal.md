### Summary
A suggestion for a simple 3d functionality extension for Gamercade. 
### Motivation

At its core, Gamercade will remain a Neo-Retro Fantasy Console. We define Neo-Retro as:

>  In our definition, it means something which is retro-looking, but still has many capabilities of modern times. In the case of Gamercade, this means that the games themselves are retro-like in their appearance, such as with lower resolutions and pixel graphics.

Since Gamercade targets the mid 90s to early 2000s home game consoles, it would be realistic to also support a basic form of 3d. Consoles of that generation include the Sega Saturn, PlayStation, and Nintendo 64. These consoles featured specific hardware to accelerate 3d graphics compared to the software implementations of the previous generation. As Gamercade is a Neo-Retro fantasy console, it should match the capabilities of those consoles. Those consoles are severely outclassed by modern hardware, but they still created engaging and memorable experiences.

Of course, the complexity jump from 2d to 3d is large, but the added dimension opens up more substantially options for modern day creatives. As Gamercade currently only natively supports 2d, developers are somewhat restricted in the kinds of experiences they can create. Adding the third dimension opens up the console to showcase the talent of 3d artists like modelers, riggers, and animators. The recent popularity of low poly artwork and stylized games show that there is still large appeal for graphically simplistic games. This is further enhanced by the faster workflow and simpler development process of these art styles. 

Example References:
[Compound VR](https://www.youtube.com/watch?v=x0N56nCqLu4) - 3d FPS VR game with pixel art textures, no lighting.
[Sketchfab #256fes Models](https://sketchfab.com/search?q=256fes&type=models) - Models from the 256fes hashtag on twitter. 256 triangle limit with 256x256 textures
[BlockBench](https://www.blockbench.net/) - Low Poly 3d modeling software
[Crocotile3d](https://crocotile3d.com/) - 3d Modeling software for making models and environments with tiled textures.
Any Nintendo 64 or PlayStation game.
### Goals & Targets
#### Primary Goals
The primary goals focus on simplicity, approachability, and usability
##### Basic 3d Rendering
We should provide a fixed-function rendering pipeline with a few adjustable knobs. Removal of user-defined shaders drastically eases the introduction into 3d. Use easy to understand shading models like vertex-colors, basic texture maps, flat shading, or Blinn-Phong shading. Lighting should be limited to a few basic light types, and the number of lights should also be limited.
##### Simple 3d API Extension
Similar to the 2d API via `GraphicsParameters` and `sprite` functions, simple one-liners or a small number of function calls to draw 3d objects. Users should not need to worry about completely understanding the graphics pipeline, cameras, or GPU memory layout, and should instead be provided with a solid set of defaults to be tweaked when necessary.
##### Editor Improvements & Accurate Viewer
The editor should represent exactly what the models would look like in-game. This means adjustments to the editor to support 3d rendering of models with animations, file importers, and bundler updates. Using a WYSIWYG workflow (similar to the Sound Engine) ensures an accurate, simple, and easily understandable representation of assets.
#### Non-Goals

##### Photorealism
Realistic shaders and high definition graphics are not necessary. They would severely increase performance requirements, increase difficulty in asset creation, and go against the Neo-Retro focus of the console. Additionally, the texture sizes needed to support this would take up much of the ROMs capacity.
##### Scene/Level Editor
Gamercade is not intended to be a game engine. The concept of a scene or level are up to the decisions of the developer. Developers wanting to make use of other (or their own) tools to assist in Gamercade development can make use of the `datapack` feature set.
##### High Performance
While we are not specifically aiming for bad performance, this 3d proposal just covers basic functionality and "good enough" performance. Due to the constraints on ROM size, its likely that most modern GPUs and iGPUs will be able to handle whatever we can throw at it without much effort.
##### Full Rendering Pipeline Access
Due to the complexity of the full rendering pipeline, we should not expose this to users. For example, wanting to support custom vertex shaders or fragment shaders means we need to consider data layout on the GPU itself, or some kind of reflection/parsing of the shader code to ensure provided parameters get to where they need to.
### Implementation Details
#### Change Summary
- Adjustments to the ROM to include optional 3d Assets
	- Support Models and Textures
	- "Global Rendering Settings"
- Creation of Gamercade 3d Library
	- 3d Rendering of Meshes
	- Skeletal Animation
	- Model, Texture, Animation loading capabilities
	- Import functionality for 3d assets
- Add required API functions to:
	- Console runtime
	- gamercade_rs
	- API Docs

#### Feature Set Proposal
##### Graphics Engine Specs & Limitations
The following limitations are built with the core principals in mind: they should be simple yet flexible, and support the Neo-Retro goal of the project. Some of these features are influenced by modern day standards (such as tangent space normal maps, and 4-bone influence animation), other constraints will keep things back within the retro feel.

- Textures
	- 32bit RGBA
	- 512x512 max size (~1mb)
	- Nearest Filtering Only
	- Repeating/Tiling Only
- Lighting 
	- Single Flat Ambient Light Value
	- Up to 4 of:
		- Directional Light
		- Point Light
		- Spotlight
	- Lights can be adjusted freely at runtime
	- Fixed Light Rendering Models
		- Unlit
		- Flat Shading
		- Blinn-Phong Shading
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
- Skeletal Animation
	- Keyframed Animation
	- Support for Location, Rotation, and Scale
	- Up to 4 Bone Influences per Vertex

### Potential Issues and Mitigation
#### Size of 3d Assets vs ROM Size Limits
The inclusion of 3d models and their accompanying data (textures, vertex colors, animation and skinning data) will heavily increase storage needs within the ROM. We should place some limits on the assets themselves, such as maximum vertex counts, triangle counts, or texture sizes.
#### 3d Rendering and Lighting
It's quite rare to see unlit 3d games, and this is likely the reason for such a severe complexity jump from 2d -> 3d. We should limit the number and kinds of lights that the engine supports to keep things simple and straightforward.
### Possible Long Term Additions

#### 3d Data Access in Code
Allow accessing the 3d data used for rendering, such as uploading meshes or textures to the GPU. This opens up the option of procedurally generated content, such as terrain, or even procedural meshes and textures to be used for rendering later.
#### Graphics Engine Tweaking
Allow users to adjust simple settings like texture filtering, mirroring, or repeating behavior, or animation changes like tweaking the maximum bone count per vertex to values between 1 and 4.
##### Animation Blending
Support blending for animations between 2 (or N) number of states.
#### Custom Shaders
Expose the shaders out to developers to allow them to write their own shaders for custom effects.
#### Performance Oriented API Additions
Mesh instancing, automated batching, etc, for advanced users who want to push the limit.
