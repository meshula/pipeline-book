# The Rendering Pipeline

In order to support the modular pipeline, you also need to update what your rendering pipeline does. I'm going to quickly run through the kinds of rendering models you typically find in game engines.

## State and Geometry Driven Renderers

State and Geometry Driven Renderers abstract an ideal underlying hardware model through their APIs. Typical examples of course are DirectX or OpenGL. The programmer is responsible for managing that virtual model of the hardware. The data that moves through the pipeline is state packets and geometry. It's the programmer's job to make this stuff all sing. The focus is on performance, not on content iteration. It can be mitigated by creating a sandbox, a place for artists to put unoptimized data to play with.

## Scene Management Based Renderers

Another common architecture is a scene management engine, sometimes known as a retained mode model, or a scene-graph. These architectures are very intrusive into your application and typically have very rigid data structures. These architectures impose a lot of structure on your game, and force their notions of the scene onto the game.

The content pipeline becomes tightly coupled to the rendering library, because you have to introduce nodes and their dependencies and things like that without which the data and the API won't actually function. The game can become slaved to the rendering library and your pipeline can sort of seize up. Changes to a rendering API can force pipeline changes, inducing massive reexport passes.

A mitigation is format conversion tools, but these are often brittle and bug-prone. They do let you go back and fix things that go wrong, but you really don't want to go there.

## Shader System Based Renderers

Shader based systems are common in film, typified by pipelines built around Renderman. A scene is described not so much as objects moving around through the system with a scene-graph and a transformation hierarchy, but instead in terms of light transport as in Renderman which implements Kajiya's model of participating media, surfaces and lights.

Shaders impact the pipeline by forcing a uniform exposure of the lighting model. This is a restriction, but it is a step in the right direction. Well designed HLSL FX pipelines hit that with well designed semantics. You can decouple the underpinnings of how things actually get down to the final product. In this case the pipeline management impact is restricted to the assignment of shaders to objects. This becomes a problem when there is something like a giant mesh that needs to be broken down into bits for simulation or something and a later computation step needs all those bits. If someone changes the mesh, maybe adds a few more verts, it might be necessary to submesh the mesh differently to retain optimality. Data farther down the pipeline has been broken. A mitigation is dependency tracker and tools that might be able to ping artists and tell them they have to fix it, or in the best case automatically detect that something upstream has changed and sort it out.

## Submission Engines

Submission engines are a completely different beast. The rendering of the scene is completely abstracted away from the engine. Lists of things to render are presented to the renderer every frame. Lists are separated by composition operators. The operators are those described in Porter and Duff, over and blend and so on, as well as sort constraints such as depth order, or alpha order. It becomes the responsibility of the submission engine to manage the underlying state of the virtual hardware and to order things correctly and optimally. The engine uses frame to frame coherence to keep things running as fast as possible; it knows what it knew last frame, and recycles as much computation as possible without programmer intervention from the application layer.

The implication for the modular pipeline stems from having decoupled the scene structure from the rendering structure, and state management from the content.