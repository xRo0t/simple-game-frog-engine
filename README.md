# Simple Game - OOP 3D

Frog OOP node-tree sandbox using Frog's independent Vulkan GPU renderer.

The current target is a standalone Frog sample: free camera, large earth
plane, clustered goblin bot nodes, group behavior, flee behavior,
and GPU-backed Vulkan mesh rendering. Rendering is handled by Frog's own
Vulkan layer. User code stays small; renderer setup, node allocation,
pointer tables, frame rendering, and cleanup are hidden behind Frog's
`Engine` API.
The base node model lives in the Frog engine package under
`packages/frog/scene/node.dlt`, so the sample code uses engine-level nodes
instead of defining its own base scene type.

```dolet
import std
import frog
import game.runtime

Engine.window.create("Simple Game - OOP Nodes", 960, 540)
Engine.properties.set_vsync(1)
Assets.mount("assets")
scene: Scene = Scene.create()

player: Node3D = Node3D.create("Player")
player_model: Model3D = Assets.models.load("models/player.gltf")
player_body: Node3D = Node3D.create("PlayerBody")
camera: Camera3D = Camera3D.create("Camera")
ground: Node3D = Node3D.create("Ground")

player_body.set_model(player_model)
player.add_child(player_body)
scene.add_child(player)
player.add_child(camera)
scene.add_child(ground)

Engine.scene.set(scene)
Engine.run()
```

Build:

```powershell
New-Item -ItemType Directory -Force -Path build
doletc .\src\main.dlt -o .\build\frog-player.exe
```

Run:

```powershell
.\build\frog-player.exe
```

Controls: mouse look, `WASD` move, arrow keys rotate/look as fallback,
`Space/E` up, `Ctrl/Q` down, `Shift` runs, `Escape` releases mouse,
`M` captures mouse again.

Rendering now goes through Frog's `Engine` facade. The Vulkan backend details
stay inside the Frog package runtime layer, not in game code.

Use `Engine.properties.set_vsync(1)` before `Engine.run()` for synced
presentation, or `Engine.properties.set_vsync(0)` for uncapped / low-latency
presentation when the driver supports it.
