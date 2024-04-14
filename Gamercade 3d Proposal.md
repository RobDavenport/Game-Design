### Summary
A suggestion for a simple 3d functionality extension for Gamercade. 

### Motivation

At its core, Gamercade will remain a Neo-Retro Fantasy Console. We define Neo-Retro as:

>  In our definition, it means something which is retro-looking, but still has many capabilities of modern times. In the case of Gamercade, this means that the games themselves are retro-like in their appearance, such as with lower resolutions and pixel graphics.

Since Gamercade targets the mid 90s to early 2000s home game consoles, it would be realistic to also support a basic form of 3d. Consoles of that generation include the Sega Saturn, PlayStation, and Nintendo 64. These consoles featured specific hardware to accelerate 3d graphics compared to the software implementations of the previous generation. As Gamercade is a Neo-Retro fantasy console, it should match the capabilities of those consoles. Those consoles are severely outclassed by modern hardware, but they still created engaging and memorable experiences.

Of course, the complexity jump from 2d to 3d is large, but the added dimension opens up more substantially options for modern day creatives. As Gamercade currently only natively supports 2d, developers are somewhat restricted in the kinds of experiences they can create. Adding the third dimension opens up the console to showcase the talent of 3d artists like modelers, riggers, and animators. The recent popularity of low poly artwork and stylized games show that there is still large appeal for graphically simplistic games. This is further enhanced by the faster workflow and simpler development process of these art styles. 

### Goals & Targets

#### Primary Goals

The primary goals focus on simplicity, approachability, and usability
##### Basic 3d Rendering
We should provide a fixed-function rendering pipeline with a few adjustable knobs. Removal of user-defined shaders drastically eases the introduction into 3d. Use easy to understand shading models like vertex-colors, basic texture maps, or Blinn-Phong shading. 
##### Simple 3d API Extension
Similar to the 2d API via `GraphicsParameters` and `sprite` functions, simple one-liners or a small number of function calls to draw 3d objects. Users should not need to worry about completely understanding the graphics pipeline, cameras, or GPU memory layout, and should instead be provided with a solid set of defaults to be tweaked when necessary.
##### Editor Improvements & Accurate Viewer
The editor should represent exactly what the models would look like in-game. This means adjustments to the editor to support 3d rendering of models with animations, file importers, and bundler updates.
#### Non-Goals

##### Photorealism
Realistic shaders and high definition graphics are not necessary. They would severely increase performance requirements, increase difficulty in asset creation, and go against the Neo-Retro focus of the console. Additionally, the texture sizes needed to support this would take up much of the ROMs capacity.
##### Scene/Level Editor
Gamercade is not intended to be a game engine. The concept of a scene or level are up to the decisions of the developer. Developers wanting to make use of other (or their own) tools to assist in Gamercade development can make use of the `datapack` feature set.
##### High Performance
While we are not specifically aiming for bad performance, this 3d proposal just covers basic functionality and "good enough" performance. Due to the constraints on ROM size, its likely that most modern GPUs and iGPUs will be able to handle whatever we can throw at it without much effort.
##### Full Rendering Pipeline Access
Due to the complexity of the 

### Implementation Details

#### Core Changes
- Adjustments to the ROM to include optional 3d Assets
#### Console & Runtime Changes
- Add the require

#### Editor Changes



### Issues and Mitigation


### Further Expansion


