# RayneEngine

![RayneEngine Cover](https://github.com/user-attachments/assets/98536ef6-3e47-4e01-a0af-c2e264530ad6)

![Version 0.1.0](https://img.shields.io/badge/Version-0.1.0-blue.svg)
![C++20](https://img.shields.io/badge/Language-C%2B%2B20-blue.svg)
![SFML 2.6](https://img.shields.io/badge/Framework-SFML_2.6-darkgreen.svg)
![Lua 5.4](https://img.shields.io/badge/Scripting-Lua_5.4_%2B_sol2-purple.svg)
![JSON](https://img.shields.io/badge/Serialization-nlohmann__json-orange.svg)
![CMake](https://img.shields.io/badge/Build-CMake_%E2%89%A5_3.16-brightgreen.svg)
![Platform](https://img.shields.io/badge/Platform-Windows_%7C_Linux-lightgrey.svg)

> **Disclaimer**: This project, or portions of its code and assets, were created with the assistance of Artificial Intelligence (AI).

---

## Overview

**RayneEngine** is a modular 2D game engine built with modern C++20 and [SFML](https://www.sfml-dev.org/). Designed as an in-depth portfolio project during game engineering training, it focuses on exploring clean software design patterns, high performance, and core engine subsystems from first principles:

- Custom, cache-conscious **Entity Component System (ECS)** supporting up to 5 000 entities
- **2D Rigidbody Physics System** with impulse-based collision response, friction, triggers, and raycasting
- **Template (Prefab) System** for saving reusable entity blueprints and instantiating them dynamically
- **Parent-Child Entity Hierarchy** with local and world transform propagation and drag-and-drop tree editing
- **Project Hub** first-run setup modal for easily configuring project settings and standalone target specs
- Native **Visual Level Editor** with live property inspection, Inspector Lock, smooth scrolling, grid snapping, and undo/redo
- **Script Export Variables** supporting `Image`, `Vec2`, `Color`, `Entity`, and `Template` with live thumbnail previews and color picker
- Dedicated **UI Editor** for designing game HUDs visually on a fixed 1920×1080 canvas
- **In-Editor Console Panel** with live stdout/stderr capture, scrollable log, and command input
- Embedded **Lua 5.4 scripting environment** via `sol2` with lifecycle events and hot-reload
- Multi-channel **Audio Engine** and hardware input polling
- Complete **JSON Scene & UI Serialization** and project auto-saving
- **UI Manager** for runtime Text, Panel, Button, Checkbox, Slider, ProgressBar, and TextInput elements controlled from Lua
- **Standalone Game Export** creating an optimized `RayneGame` executable using your project settings

---

## Recent Changelog

### Added
- **2D Rigidbody Physics System & Impulse Solver:**
  - Added `Rigidbody2DComponent` with configurable `bodyType` (`Dynamic`, `Kinematic`, `Static`), `mass`, `gravityScale`, `restitution` (bounciness), `drag` (linear damping), and `freezeRotation`.
  - Added fixed-timestep physics simulation loop with sub-stepping (`PhysicsSystem`).
  - Added impulse-based physical collision resolution with coefficient of restitution and tangential surface friction.
  - Added trigger / sensor collider support (`isTrigger`) with zero-impulse passthrough and event dispatch.
  - Added physics lifecycle callbacks in Lua: `OnCollisionEnter(self, other, normalX, normalY)` and `OnTriggerEnter(self, other)`.
  - Added full Lua `Physics` library: `Physics.Raycast`, `Physics.ApplyForce`, `Physics.ApplyImpulse`, `Physics.SetVelocity`, `Physics.GetVelocity`, `Physics.SetGravity`, `Physics.GetGravity`, `Physics.SetFixedTimestep`, and `Physics.GetFixedTimestep`.
  - Added global entity Rigidbody Lua helpers: `AddRigidbody(entity, bodyType, mass, gravityScale)`, `GetRigidbody(entity)`, and `HasRigidbody(entity)`.
  - Added dedicated Rigidbody 2D property inspector with real-time controls, body type selectors, and rotation locks.
- **Inspector Lock Feature:**
  - Added Unity-style lock toggle button in the top-right corner of the Inspector panel.
  - Freezes the active inspector view on the selected object so clicking other entities in the viewport or hierarchy does not deselect or alter the inspector view.
- **Script Export Variables Expansion & Visual Previews:**
  - Added first-class export variable constructors in Lua: `Image(path)`, `Vec2(x, y)`, `Color(r, g, b)`, `Entity(name)`, and `Template(path)`.
  - Added live thumbnail rendering directly inside the Inspector for `Image` texture assets and `Template` prefabs.
  - Added native Windows color picker integration (`ChooseColor`) for `Color` export properties.
  - Added drag-and-drop targeting: drag images and templates from the Content Browser, or entities from the Hierarchy tree, directly into matching export property slots.
  - Added complete JSON serialization and deserialization for all script export property types.
- **Inspector Scrolling & Content Clipping:**
  - Added smooth mouse-wheel scrolling for the Inspector panel when content exceeds viewport height.
  - Added viewport scissor clipping below the fixed title header to keep content cleanly contained within the panel.
- **Template (Prefab) System:**
  - Added support for saving entity hierarchies as reusable `.template` asset files.
  - Added template instantiation from Lua via `Instantiate(template, x, y, [parent])` and `Template(path):Instantiate(...)`.
  - Added circular reference detection and nested prefab dependency warnings.
- **Parent-Child Hierarchy System:**
  - Added complete transform parenting system (`HierarchySystem`) supporting local and world coordinate spaces.
  - Added Lua hierarchy APIs: `SetParent(child, parent, [keepWorldTransform])`, `GetParent(child)`, `GetChildren(parent)`, `GetWorldPosition(e)`, `GetWorldRotation(e)`, and `GetWorldScale(e)`.
  - Added drag-and-drop reparenting inside the Hierarchy tree with visual feedback.
  - Added unsaved changes dirty indicator (`*`) across scenes, UI layouts, and scripts.
- **UI Editor Enhancements & Layout Containers:**
  - Added `VerticalBox` and `HorizontalBox` layout elements with automatic element spacing and alignment.
  - Added progress bar elements with `SetProgressValue` / `GetProgressValue`.
  - Added slider min/max boundary configuration (`SetSliderMin`, `SetSliderMax`, `GetSliderMin`, `GetSliderMax`).
  - Added vertical text alignment (`SetTextVAlign`) and auto-wrapping labels in the UI inspector.
  - Added UI element focus management (`SetFocused`, `GetFocused`, `OnUIFocus`).
  - Added script method stub insertion, asset synchronization, and IDE workflow directly from the UI Editor.
- **Editor Tooling & Visual Feedback:**
  - Added viewport world coordinate axes and origin marker (0, 0).
  - Added automatic scene saving on Play Mode launch (`F5`).
  - Added hot-reloading and inspector property refreshing for exported script variables.
  - Added on-screen notification toasts via `Engine.LogToScreen` and `LogToScreen` during play mode.
  - Added color-coded console badges for `Engine.Log` (teal), `Engine.LogWarning` (amber), and `Engine.LogError` (red).
  - Added `.jfif` image format loading support.
  - Added read-only file attributes for engine content and template files in standalone builds.

### Fixed
- Fixed text underline disappearing when bold style is active in UIManager text rendering.
- Fixed object rotation handle positioning and smooth angular dragging in the level editor viewport.
- Fixed hit detection and selection on scaled entities to prevent miss-clicks and inaccurate marquee selection.
- Fixed non-uniform edge scaling and ellipse stretching for circle primitives across both editor and runtime.
- Fixed IDE launch command to run detached without spawning extraneous console windows.
- Fixed UI button click handling in standalone builds and ensured all audio channels stop when exiting play mode.
- Fixed startup freeze/hang in `EditorScene` and revealed hidden scripting directories in build mode.
- Fixed template asset path resolution to reliably locate project root using `FindProjectRoot()`.

### Removed
- Removed legacy non-Lua comment cruft from `EditorScene`, `ContentBrowser`, `PhysicsSystem`, `SplashScreen`, and scripting manager classes.
- Removed redundant `SetPathReadOnly` calls from `EditorScene` constructor.

---

## Architecture

The engine architecture is structured into decoupled modules under `src/Core/`:

RayneEngine|-- Core/|   |-- Application/      Main loop, DPI awareness, window management, engine versioning|   |-- Audio/            AudioManager sound effect pool (32 channels) and music streaming|   |-- ECS/              Registry, component pools, view iterators, entity handles,|   |                     PhysicsSystem (Rigidbody2D solver), HierarchySystem (parenting)|   |-- Input/            InputManager keyboard & mouse tracking with frame edge detection|   |-- Math/             MathR custom math routines, interpolation, trigonometric tables; Vector3|   |-- Primitives/       Geometric primitive factories (Rectangles, Circles, Polygons)|   |-- Resources/        ResourceManager centralized caching (textures, fonts, sounds)|   |-- Scenes/           SceneManager, EditorScene, UIEditorScene, GameScene,|   |                     ContentBrowser, ConsolePanel, SceneSerializer|   |-- Scripting/        LuaState bindings, ScriptComponent lifecycle, EventManager,|   |                     TimerManager, TweenManager, api_stub.lua|   -- UI/               UIManager (Text, Panel, Button elements, JSON persistence) -- assets/               Scenes, scripts, audio files, fonts, textures, and UI layouts
![RayneEngine Editor Interface](https://github.com/user-attachments/assets/939ad1b6-3de2-44d1-97d1-38adfb8ab955)

---

## Subsystems and Features

### Project Hub (First-Run Setup)

When launching the engine for the first time (i.e. no `project_settings.json` is found), the **Project Hub** will appear as a sleek modal dialog over a dimmed editor background:
- **Project Details:** Define the project name and author.
- **Resolution:** Set the target window width and height for your standalone game export.
- **Engine Settings:** Toggle VSync and specify a target framerate (FPS).

These settings are serialized to `project_settings.json`, governing standalone runtime behavior without interfering with the editor's fixed resolution.

![StartupScreenRE](https://github.com/user-attachments/assets/8e7e0c96-a6a6-4168-858f-dd590b5f1f5e)

---

### Visual Level Editor (`EditorScene`)

The built-in level editor provides a real-time environment for constructing and previewing 2D game scenes:

- **Entity Hierarchy & Parenting:**
  - Scrollable hierarchy tree displaying all active scene entities.
  - **Transform Parenting:** Drag-and-drop entities onto other objects to establish parent-child relationships with automatic local/world coordinate propagation.
  - Context menu actions: Rename, duplicate, delete, and save entity subtrees as `.template` assets.
  - Multi-selection and group hierarchy operations.
- **Property Inspector:**
  - **Inspector Lock:** Unity-style lock button in the header freezes inspection on the current entity, preventing accidental selection changes when clicking in the viewport or hierarchy.
  - **Smooth Scrolling & Viewport Scissor:** Mouse-wheel scrolling with view clipping ensures large component lists and extensive export variables remain completely accessible.
  - **Rigidbody 2D Section:** Real-time configuration of `BodyType` (Dynamic / Kinematic / Static), `Mass`, `Gravity Scale`, `Restitution` (bounciness), `Drag`, and `Freeze Rotation`.
  - **Script Export Variables:** Auto-generates UI fields for all exported script properties (`Int`, `Float`, `Bool`, `String`, `Image`, `Vec2`, `Color`, `Entity`, `Template`).
  - **Visual Asset Previews:** Real-time thumbnail rendering for `Image` texture assets and `Template` prefab files.
  - **Windows Color Picker:** Interactive color swatches that launch the native Windows `ChooseColor` dialog.
  - **Drag-and-Drop Slots:** Drop textures and templates from the Content Browser, or entities from the Hierarchy tree, directly into matching property slots.
  - Live numerical manipulation of position (`x`, `y`), size (`width`, `height`), rotation (degrees), and scale (`scaleX`, `scaleY`).
  - Entity Tag and Name assignment for easy query from Lua scripts.
  - Color palette tinting (`R`, `G`, `B`) for primitives and sprites.
  - Script path assignment with automatic Lua binding and hot-reload reflection.
  - Sprite asset assignment with aspect-correct scaling.
  - Collision channel, collision type (`Static` / `Solid`), collider shape (`Box` / `Circle`), and sensor trigger (`isTrigger`) configuration.
  - UI element text editing directly from the inspector (`UIText` field).
- **Content Browser:**
  - Integrated file browser with breadcrumb navigation and path history.
  - Asset category filters: All, Images, Scripts, Audio, Scenes.
  - Live search bar with instant filtering across all entries.
  - File management: Create scripts, scenes, or folders; rename, duplicate, or delete assets; copy asset path to clipboard; reveal file in OS file explorer.
  - Drag-and-drop: Drag textures or scripts from the browser directly onto viewport entities.
  - Scene loading: Double-click or trigger scene loading requests directly from the browser.
- **In-Editor Console Panel:**
  - Switchable bottom panel (Content Browser ↔ Console via tab bar).
  - Captures all `std::cout` and `std::cerr` output in real time via stream redirectors installed at engine boot.
  - Color-coded log lines: normal output in light grey, errors in red.
  - Scrollable log area with draggable scrollbar and mouse-wheel support; auto-scrolls to the newest message.
  - Command input field with blinking cursor, Ctrl+V paste support, and Enter-to-submit.
  - Built-in `clear` command to reset the log. Buffer capped at 2 000 messages.
- **Interactive Viewport & Selection:**
  - Free camera panning using Middle Mouse Button (invertible and sensitivity-configurable).
  - Smooth camera zooming with user-defined min/max limits.
  - **Multi-Selection & Box-Select:** Drag in empty space to create a selection box (marquee select); hold `Shift` or `Ctrl` to toggle entities in the selection group.
  - **Group Manipulation:** Move or delete multiple selected entities simultaneously.
  - **Undo / Redo (Command Pattern):** Full history stack supporting `Ctrl + Z` and `Ctrl + Y` for entity creation, deletion, dragging, and resizing — including atomic `MacroCommand` grouping for multi-object operations.
  - 8-point interactive resize handles for live scaling of selected objects.
  - Grid rendering with configurable cell dimensions, color, and opacity.
  - Toggleable snap-to-grid alignment.
- **Scene Serialization:**
  - Completely relative asset paths: Sprite textures and script files are saved relative to `assets/` for cross-platform and team portability.
- **Auto-Save & Configuration:**
  - Configurable auto-save intervals with on-screen notification popups.
  - Persistent JSON-based editor settings dialog covering general, editor, camera, and debug parameters.
- **Debug Overlays:**
  - Real-time FPS monitoring with configurable framerate limits.
  - Collider outline rendering for rapid physics debugging.
  - Entity ID labels drawn in world space.

---

### UI Editor (`UIEditorScene`) & UI Manager (`UIManager`)

A dedicated scene for visually designing game HUDs and UI layouts:

- **Fixed 1920×1080 canvas:** Matches runtime UI view for pixel-perfect WYSIWYG design.
- **Element Types:** `Text`, `Panel`, `Button`, `Checkbox`, `Slider`, `ProgressBar`, `TextInput`, `VerticalBox`, `HorizontalBox`.
- **Canvas Interaction:** Select, drag, and resize elements using 8-point handles; middle-mouse pan across canvas.
- **Element Hierarchy:** Scrollable panel listing all UI elements with parent-child nesting support.
- **Property Inspector:** Live editing of 40+ fields including position, size, z-index, opacity, color (RGBA), text content, font size, letter/line spacing, text alignment (`Left`, `Center`, `Right`), text style bitmask (Bold, Italic, Underline, StrikeThrough), text outline, button normal/hover/pressed/disabled colors and textures, and panel borders.
- **Persistence:** UI layouts are saved to and loaded from `assets/ui.json` via `UIManager`.
- **Lua Integration:** All elements, properties, and interactions can be queried and modified at runtime.

---

### Entity Component System (ECS)

The custom ECS emphasizes data locality, cache friendliness, and clean decoupling. The registry supports up to **5 000 entities** (`MAX_ENTITIES = 5000`):

- **`Registry`:** Manages entity lifecycles (`CreateEntity`, `DestroyEntity`, `Clear`) and hosts type-safe component pools. Entity IDs start at `1`; `NULL_ENTITY = 0`.
- **`Pool<T>`:** Contiguous memory pools storing component instances with sparse-to-dense mappings for fast iteration (swap-and-pop removal).
- **`View<Components...>`:** Multi-component query views enabling `Registry::ForEach<T1, T2>(...)` iteration patterns via range-based for loops.
- **Available Components:**
  - `TagComponent`: Name and classification tag (`std::string tag`).
  - `TransformComponent`: 2D spatial position (`x`, `y`), rotation angle in degrees (`rotation`), and scaling (`scaleX`, `scaleY`). Supports parent-child hierarchies with world coordinates.
  - `VelocityComponent`: Movement velocity delta (`dx`, `dy`).
  - `Rigidbody2DComponent`: Physics body attributes (`bodyType` Dynamic/Kinematic/Static, `mass`, `gravityScale`, `restitution`, `drag`, `freezeRotation`).
  - `RenderComponent`: Visual representation (`sf::Color`, `sf::Vector2f size`, and `ShapeType`: Rectangle, Circle, Triangle, Pentagon, Hexagon).
  - `SpriteComponent`: Renderable SFML sprite with texture handle and dimensions.
  - `CameraComponent`: Marks an entity as the active camera focus (`bool active`).
  - `CollisionComponent`: Configures collision filtering via integer `channel`, collision type (`Static`, `Solid`), collider shape (`Box`, `Circle`), and trigger flag (`isTrigger`).
  - `ScriptComponent`: Encapsulates a sol2 Lua environment, filesystem modification timestamp tracking for live hot-reloading, exported variables, and lifecycle hooks.

---

### Physics & Event Pipeline

- **Physics Simulation (`PhysicsSystem`):**
  - **Fixed Timestep Loop:** Accumulator-based fixed-timestep integration with sub-stepping (`Physics.SetFixedTimestep(dt)`).
  - **Gravity Acceleration:** Configurable global gravity vector (`Physics.SetGravity(gx, gy)`). Default: `(0, 980)`.
  - **Linear Velocity Damping:** Applies aerodynamic/frictional drag per body (`drag`).
  - **Body Types:**
    - `BodyType::Dynamic` — Full physics simulation responding to gravity, forces, impulses, and contact collisions.
    - `BodyType::Kinematic` — Controlled programmatically via velocity; pushes dynamic objects without displacement.
    - `BodyType::Static` — Immovable obstacles; infinite mass in collision responses.
  - **Rotation Lock:** Toggle `freezeRotation` to prevent angular tumble on impact.
- **Impulse Solver & Friction:**
  - Physical collision resolution applies restitution-weighted impulses `j = -(1 + e) * v_rel / (invMassA + invMassB)`.
  - Realistic tangential friction damping (`frictionMu = 0.2`).
- **Sensor / Trigger Colliders:**
  - Any collider flagged with `isTrigger = true` bypasses impulse resolution entirely and fires `OnTriggerEnter(self, other)`.
- **Collision Callbacks:**
  - `OnCollisionEnter(self, other, normalX, normalY)` dispatches once upon solid contact with the contact normal.
  - `OnTriggerEnter(self, other)` dispatches when entering a sensor zone.
  - `OnCollision(self, other)` legacy fallback event dispatched on contact.
- **Raycasting:**
  - `Physics.Raycast(startX, startY, dirX, dirY, distance, [channel])` performs ray intersection against active scene colliders.

---

### Engine Versioning

The `Rayne` namespace in `EngineVersion.h` provides compile-time constants and helpers:

- `Rayne::VERSION_MAJOR / MINOR / PATCH` — Current version (`0.1.0`)
- `Rayne::VersionString()` — Returns `"0.1.0"`
- `Rayne::PlatformString()` — Returns `"Windows"` / `"macOS"` / `"Linux"` / `"Unknown"`
- `Rayne::DEFAULT_PROJECT_NAME` — Default window title prefix used in Play Mode

---

## Editor Hotkeys and Controls

| Shortcut / Input | Context | Action |
|---|---|---|
| `F5` | Editor | Run simulation in Play Mode (`GameScene`) |
| `F6` | Game Mode | Hot-reload all modified Lua scripts dynamically |
| `Escape` | Game Mode | Return to Editor Mode |
| `Escape` | Editor | Cancel text input, close menus, or deselect active entity |
| `Ctrl + Z` | Editor | Undo last action (create, delete, move, resize) |
| `Ctrl + Y` | Editor | Redo last undone action |
| `Ctrl + S` | Editor | Save current scene to JSON (using relative asset paths) |
| `Ctrl + L` | Editor | Reload current scene from JSON |
| `Ctrl + D` | Editor | Duplicate selected entity with offset |
| `Ctrl + ,` | Editor | Open / Close Editor Settings modal |
| `G` | Editor | Toggle grid snapping on / off |
| `Delete` | Editor | Delete all currently selected entities |
| `Middle Mouse Drag` | Editor | Pan camera viewport |
| `Mouse Scroll Wheel` | Editor | Zoom camera in / out |
| `Left Click (Entity)` | Editor | Select entity and drag to reposition |
| `Shift / Ctrl + Click`| Editor | Add or toggle entity in multi-selection group |
| `Left Click + Drag (Empty)` | Editor | Box-select (marquee) multiple entities in viewport |
| `Left Click (Handles)`| Editor | Resize entity along 8 anchor handles |
| `Left Click (Empty)` | Editor | Place new primitive shape of selected type |

---

## Complete Lua Scripting API

RayneEngine embeds Lua 5.4 using `sol2`. Every script attached to an entity receives its own isolated environment with the entity handle available through the global `self` variable. Scripts are automatically monitored and hot-reloaded at runtime upon file modification.

### Lifecycle Callbacks

```lua
function OnCreate(self)
    -- Invoked once when the entity is instantiated in the scene
end

function OnUpdate(self, dt)
    -- Invoked every simulation frame; dt represents delta time in seconds
end

function OnCollisionEnter(self, other, normalX, normalY)
    -- Invoked upon solid contact with another entity, providing the collision normal
end

function OnTriggerEnter(self, other)
    -- Invoked when entering a trigger/sensor collider zone
end

function OnCollision(self, other)
    -- Legacy fallback invoked upon collision contact
end

function OnDestroy(self)
    -- Invoked when the entity or scene is destroyed
end
UI Event CallbacksLuafunction OnButtonClicked(id) end
function OnButtonHovered(id) end
function OnSliderChanged(id, value) end
function OnCheckboxChanged(id, checked) end
function OnTextInputChanged(id, text) end
function OnTextInputSubmitted(id, text) end
function OnUIHover(id, hovered) end
function OnUIFocus(id, focused) end
Script Export VariablesRayneEngine allows scripts to expose typed variables to the Editor Inspector using an Export table and built-in type constructors:Lua-- ExportDemoScript.lua
characterTexture = Image("assets/sprites/character.png") -- Texture asset path (thumbnail preview)
spawnOffset      = Vec2(50.0, -20.0)                     -- 2D Vector (X, Y fields)
tintColor        = Color(255, 128, 64)                   -- RGB Color (Windows color picker)
targetObject     = Entity("Player")                      -- Entity reference (drag from hierarchy)
spawnPrefab      = Template("assets/bullet.template")    -- Prefab path (thumbnail preview)
moveSpeed        = 120.0                                 -- Float
maxHealth        = 100                                   -- Integer
enableGlow       = true                                  -- Boolean checkbox
greetingMessage  = "Hello RayneEngine!"                  -- String

Export = {
    "characterTexture",
    "spawnOffset",
    "tintColor",
    "targetObject",
    "spawnPrefab",
    "moveSpeed",
    "maxHealth",
    "enableGlow",
    "greetingMessage"
}
Global Entity & Component APIFunctionSignatureDescriptionCreateEntity() -> EntityInstantiates a new entity and returns its integer IDDestroyEntity(e: Entity)Removes an entity and all its attached componentsAddTag(e: Entity, tag: string)Attaches a new TagComponentSetTag(e: Entity, tag: string)Attaches or updates an existing TagComponentGetTag(e: Entity) -> stringRetrieves entity tag / name stringHasTag(e: Entity) -> booleanChecks if entity has a TagComponentFindEntityWithTag(tag: string) -> EntitySearches and returns first entity with matching tag, or 0AddTransform(e: Entity, x: number, y: number)Attaches a TransformComponentGetTransform(e: Entity) -> TransformReturns a mutable reference to { x, y, rotation, scaleX, scaleY }SetPosition(e: Entity, x: number, y: number)Sets spatial position directlySetRotation(e: Entity, angle: number)Sets spatial rotation angle in degreesSetScale(e: Entity, sx: number, sy: number)Sets spatial scale factorsHasTransform(e: Entity) -> booleanChecks if entity has a transformAddVelocity(e: Entity, dx: number, dy: number)Attaches a VelocityComponentGetVelocity(e: Entity) -> VelocityReturns a mutable reference to { dx, dy }SetVelocity(e: Entity, dx: number, dy: number)Sets velocity vector directlyHasVelocity(e: Entity) -> booleanChecks if entity has velocityAddSprite(e: Entity, path: string, w: number, h: number)Attaches a SpriteComponentSetSprite(e: Entity, path: string)Updates or swaps the sprite textureSetSpriteSize(e: Entity, w: number, h: number)Updates rendered dimensions of spriteHasSprite(e: Entity) -> booleanChecks if entity has a spriteSetColor(e: Entity, r: number, g: number, b: number, [a]: number)Sets color of RenderComponent (0–255)AddCamera(e: Entity)Attaches camera tracking componentRemoveCamera(e: Entity)Removes camera componentHasCamera(e: Entity) -> booleanChecks if entity has camera trackingAddCollision(e: Entity, [channel]: integer)Attaches a CollisionComponent (default channel 0, type Static)RemoveCollision(e: Entity)Removes collision componentHasCollision(e: Entity) -> booleanChecks if entity has collision enabledSetCollisionChannel(e: Entity, channel: integer)Sets collision filter channelGetCollisionChannel(e: Entity) -> integerReads collision filter channelSetCollisionType(e: Entity, type: string)Sets collision type: "static" or "solid"GetCollisionType(e: Entity) -> stringReturns current collision type as stringSetParent(child: Entity, parent: Entity, [keepWorldTransform=true]: boolean)Sets entity parent; 0 unparentsGetParent(child: Entity) -> EntityReturns parent entity ID, or 0GetChildren(parent: Entity) -> Entity[]Returns array of child entity IDsGetWorldPosition(e: Entity) -> number, numberComputes global world coordinates x, yGetWorldRotation(e: Entity) -> numberComputes global world rotation in degreesGetWorldScale(e: Entity) -> number, numberComputes global world scale factors sx, syTemplate(path: string) -> TemplateCreates template reference objectInstantiate(template: Template | string, x: number, y: number, [parent]: Entity) -> EntityInstantiates template prefab into sceneLoadScene(sceneName: string)Switches active scene to assets/scenes/<sceneName>.jsonRestartScene()Resets and reloads the active scenePauseGame(paused: boolean)Pauses or unpauses game simulationSetTimeScale(scale: number)Sets simulation time scale factorGetFPS() -> numberReturns current frames per secondQuitGame()Exits standalone build or returns to editorLogToScreen(msg: string, [duration=3.5], [r], [g], [b])Displays on-screen toast during play mode2D Rigidbody Physics Library (Physics)FunctionSignatureDescriptionAddRigidbody(e: Entity, [bodyType], [mass=1.0], [gravityScale=1.0]) -> Rigidbody2DAttaches a Rigidbody2D componentGetRigidbody(e: Entity) -> Rigidbody2D | nilReturns the Rigidbody2D component or nilHasRigidbody(e: Entity) -> booleanChecks if entity has a RigidbodyPhysics.Raycast(startX, startY, dirX, dirY, distance, [channel=-1]) -> RaycastResultCasts a ray and returns closest hit informationPhysics.ApplyForce(e: Entity, fx: number, fy: number)Applies continuous force (accumulated for next physics step)Physics.ApplyImpulse(e: Entity, ix: number, iy: number)Applies instantaneous impulse directly modifying velocityPhysics.SetVelocity(e: Entity, vx: number, vy: number)Sets linear velocity directlyPhysics.GetVelocity(e: Entity) -> number, numberGets current linear velocity vx, vyPhysics.SetGravity(gx: number, gy: number)Sets global physics gravity vector (default: 0, 980)Physics.GetGravity() -> number, numberReturns current global gravity vectorPhysics.SetFixedTimestep(dt: number)Sets fixed simulation timestep in seconds (default: 1/60)Physics.GetFixedTimestep() -> numberReturns current fixed simulation timestepEnums:BodyType: BodyType.Dynamic (0), BodyType.Kinematic (1), BodyType.Static (2)ColliderShape: ColliderShape.Box (0), ColliderShape.Circle (1)Engine & System Library (Engine)FunctionSignatureDescriptionEngine.Log / print(...)Prints highlighted message to in-editor console with teal [LOG] badgeEngine.LogWarning(msg: string)Prints warning message with amber [WARN] badgeEngine.LogError(msg: string)Prints error message with red [ERROR] badgeEngine.LogToScreen(msg: string, [duration=3.5], [r], [g], [b])Displays on-screen notification toast during play modeEngine.SetPaused(paused: boolean)Pauses or unpauses game simulationEngine.IsPaused / Engine.GetPaused() -> booleanReturns true if simulation is currently pausedEngine.TogglePause()Toggles simulation pause stateEngine.SetTimeScale(scale: number)Sets simulation time scale factorEngine.GetTimeScale() -> numberGets current simulation time scale factorEngine.SetFullscreen(fullscreen: boolean)Sets window fullscreen modeEngine.ToggleFullscreen()Toggles window fullscreen modeEngine.IsFullscreen() -> booleanReturns true if window is currently fullscreenEngine.SetCursorVisible(visible: boolean)Shows or hides the mouse cursorEngine.TakeScreenshot([filename]: string) -> stringCaptures screenshot and saves to screenshots/Engine.OpenURL(url: string)Opens URL or file path in default OS handlerEngine.GetFPS() -> numberReturns current frames per secondEngine.GetDeltaTime() -> numberReturns unscaled delta time of current frameEngine.ShowFPS(show: boolean)Shows or hides on-screen FPS counter overlayEngine.IsFPSShown() -> booleanReturns true if FPS counter is visibleEngine.RestartScene / Engine.RestartCurrentScene()Resets and reloads the active sceneEngine.LoadScene(sceneName: string)Loads scene from assets/scenes/<name>.jsonEngine.Quit()Exits standalone build or returns to editorMath Library (MathR)FunctionSignatureDescriptionMathR.ClampI(value: number, min: number, max: number) -> numberClamps an integer between boundsMathR.ClampF(value: number, min: number, max: number) -> numberClamps a float between boundsMathR.ClampF01(value: number) -> numberClamps a float into [0.0, 1.0]MathR.AbsI(value: number) -> numberReturns integer absolute valueMathR.AbsF(value: number) -> numberReturns floating-point absolute valueMathR.Ceil(value: number) -> numberSmallest integer greater than or equal to argumentMathR.Floor(value: number) -> numberLargest integer less than or equal to argumentMathR.Lerp(start: number, endVal: number, factor: number) -> numberLinearly interpolates between two valuesMathR.InverseLerp(start: number, endVal: number, value: number) -> numberComputes interpolation factor for valueMathR.Sin(x: number) -> numberSine via 10-term Taylor series approximationMathR.Cos(x: number) -> numberCosine computationInput Library (Input)FunctionSignatureDescriptionInput.IsKeyDown(key: number | string) -> booleanTrue while key is held downInput.IsKeyPressed / Input.IsKeyJustPressed(key: number | string) -> booleanTrue during the frame key was pressedInput.IsKeyReleased(key: number | string) -> booleanTrue during the frame key was releasedInput.IsMouseDown(button: number | string) -> booleanTrue while mouse button is held downInput.IsMousePressed(button: number | string) -> booleanTrue during the frame mouse button was pressedInput.IsMouseReleased(button: number | string) -> booleanTrue during the frame mouse button was releasedInput.MouseX() -> numberMouse horizontal position in screen spaceInput.MouseY() -> numberMouse vertical position in screen spaceInput.MouseScroll() -> numberMouse wheel scroll delta for current frameKey Enums (Key / KeyCode): A through Z, Space, Enter, Escape, LShift, RShift, LCtrl, RCtrl, Left, Right, Up, Down, Tab, Delete.Key strings (case-insensitive): "a"–"z", "0"–"9", "space", "enter", "escape", "shift", "ctrl", "alt", "left", "right", "up", "down", "tab", "delete", "backspace".Mouse Enums (Mouse): Left, Right, Middle (or strings "left", "right", "middle").Audio Library (Audio)FunctionSignatureDescriptionAudio.PlaySound(path: string, [volume=100]: number, [pitch=1.0]: number)Plays sound effect via channel poolAudio.StopAllSounds()Stops all active sound channelsAudio.PlayMusic(path: string, [loop=true]: boolean, [volume=100]: number)Plays streaming background musicAudio.StopMusic()Stops background music streamAudio.PauseMusic()Pauses background music streamAudio.ResumeMusic()Resumes paused music streamAudio.SetMusicVolume(volume: number)Adjusts music volume (0–100)Audio.SetMasterVolume(volume: number)Adjusts global engine volume (0–100), rescales active soundsResource Library (Resource)FunctionSignatureDescriptionResource.PreloadTexture(path: string) -> booleanCaches a texture in memoryResource.PreloadFont(path: string) -> booleanCaches a font in memoryResource.PreloadSound(path: string) -> booleanCaches a sound buffer in memoryResource.ClearTextures()Flushes texture cacheResource.ClearFonts()Flushes font cacheResource.ClearSounds()Flushes sound cacheResource.ClearAll()Flushes entire resource cacheResource.TextureCount() -> numberNumber of cached texturesResource.FontCount() -> numberNumber of cached fontsResource.SoundCount() -> numberNumber of cached soundsResource.PrintStats()Prints memory statistics to consoleTimer & Tween LibrariesFunctionSignatureDescriptionTimer.After(delay: number, callback: function)Executes callback function once after delay secondsTween.Position(entity: Entity, targetX: number, targetY: number, duration: number, [easing]: string)Interpolates entity position over durationSupported Easing Modes: "linear", "EaseInQuad", "EaseOutQuad", "EaseInOutQuad".UI Subsystem API (UI & Global Helpers)Both object-oriented UI.* and flat global UI_* helpers are exposed to Lua:FunctionGlobal AliasDescriptionUI.IsButtonClicked(id)UI_IsButtonClicked(id)True during frame button was clickedUI.IsButtonHovered(id)UI_IsButtonHovered(id)True while cursor is over buttonUI.SetText(id, text)UI_SetText(id, text)Sets display text of Text or Button elementUI.GetText(id)UI_GetText(id)Gets current display text stringUI.SetPosition(id, x, y)UI_SetPosition(id, x, y)Sets coordinates on 1920×1080 canvasUI.SetSize(id, w, h)UI_SetSize(id, w, h)Sets element width and heightUI.SetColor(id, r, g, b, [a])UI_SetColor(id, r, g, b, [a])Sets background fill color (0–255)UI.SetZIndex(id, z)UI_SetZIndex(id, z)Sets depth layer (higher renders in front)UI.GetZIndex(id)UI_GetZIndex(id)Reads depth layerUI.SetVisible(id, visible)UI_SetVisible(id, visible)Toggles element visibility and hit-testingUI.GetVisible(id)UI_GetVisible(id)Returns element visibility stateUI.SetOpacity(id, opacity)UI_SetOpacity(id, opacity)Sets alpha transparency (0–255)UI.SetTextStyle(id, style)UI_SetTextStyle(id, style)Bitmask: 0=Reg, 1=Bold, 2=Italic, 4=Underline, 8=StrikeUI.SetTextAlign(id, align)UI_SetTextAlign(id, align)Horizontal alignment: 0=Left, 1=Center, 2=RightUI.SetTextVAlign(id, valign)—Vertical alignment: 0=Top, 1=Middle, 2=BottomUI.SetUpperCase(id, upper)UI_SetUpperCase(id, upper)Forces text display to uppercaseUI.SetFontSize(id, size)UI_SetFontSize(id, size)Sets character font size in pointsUI.SetLetterSpacing(id, val)UI_SetLetterSpacing(id, val)Sets letter spacing multiplierUI.SetLineSpacing(id, val)UI_SetLineSpacing(id, val)Sets line spacing multiplierUI.SetTextColor(id, r, g, b, a)UI_SetTextColor(id, r, g, b, a)Sets text fill color independentlyUI.SetTextOutline(id, r, g, b, a, thick)UI_SetTextOutline(...)Sets text outline color and thicknessUI.SetTextOffset(id, ox, oy)UI_SetTextOffset(id, ox, oy)Manual pixel offset for text inside elementUI.SetOutline(id, r, g, b, a, thick)UI_SetOutline(...)Sets border color and thicknessUI.SetDisabled(id, disabled)UI_SetDisabled(id, disabled)Toggles interactive disabled stateUI.SetParent(childId, parentId, [keep])UI_SetParent(...)Sets parent UI elementUI.GetParent(id)UI_GetParent(id)Gets parent element IDUI.GetChildren(id)UI_GetChildren(id)Returns array of child element IDsUI.GetWorldPosition(id)UI_GetWorldPosition(id)Computes global canvas position x, yUI.SetTexture(id, path)—Sets normal image textureUI.SetHoverTexture(id, path)—Sets button hover textureUI.SetPressedTexture(id, path)—Sets button pressed textureUI.SetCheckedTexture(id, path)—Sets checkbox checked textureUI.SetChecked(id, checked)—Sets checkbox toggle stateUI.GetChecked(id)—Returns checkbox stateUI.SetSliderValue(id, val)—Sets current slider valueUI.GetSliderValue(id)—Reads current slider valueUI.SetSliderMin(id, min)—Sets slider minimum valueUI.SetSliderMax(id, max)—Sets slider maximum valueUI.GetSliderMin(id)—Reads slider minimum valueUI.GetSliderMax(id)—Reads slider maximum valueUI.SetProgressValue(id, val)—Sets progress bar fill (0.0 to 1.0)UI.GetProgressValue(id)—Reads progress bar fill valueUI.SetFocused(id, focused)—Sets input focus on TextInput elementUI.GetFocused(id)—Checks if element is focusedUI.IsHovered(id)—Checks hover stateUI.IsPressed(id)—Checks pressed stateUI.IsDisabled(id)—Checks disabled stateComplete Lua Script ExampleLua-- Player controller demonstrating Input, Transform, Audio, Collision, and UI
local speed = 250
local bounceAmplitude = 20
local timer = 0

function OnCreate(self)
    if not HasCollision(self) then
        AddCollision(self, 0)
        SetCollisionType(self, "solid")
    end
    print("Entity " .. tostring(self) .. " spawned successfully.")
end

function OnUpdate(self, dt)
    local t = GetTransform(self)
    if not t then return end

    local moveX = 0
    local moveY = 0

    -- Input polling via string names or Key enums
    if Input.IsKeyDown("w") or Input.IsKeyDown(Key.Up) then
        moveY = moveY - 1
    end
    if Input.IsKeyDown("s") or Input.IsKeyDown(Key.Down) then
        moveY = moveY + 1
    end
    if Input.IsKeyDown("a") or Input.IsKeyDown(Key.Left) then
        moveX = moveX - 1
    end
    if Input.IsKeyDown("d") or Input.IsKeyDown(Key.Right) then
        moveX = moveX + 1
    end

    -- Smooth movement
    t.x = t.x + moveX * speed * dt
    t.y = t.y + moveY * speed * dt

    -- Trigonometric animation via MathR
    timer = timer + dt
    local bounceOffset = MathR.Sin(timer * 4.0) * bounceAmplitude

    -- Update runtime HUD
    UI_SetText("pos_label", string.format("X: %.0f  Y: %.0f", t.x, t.y))

    -- Sound effect and tween on jump key press edge
    if Input.IsKeyPressed(Key.Space) then
        Audio.PlaySound("assets/sounds/jump.wav", 80, 1.0)
        Tween.Position(self, t.x, t.y - 100, 0.3, "EaseOutQuad")
    end
end

function OnCollision(self, other)
    local tag = HasTag(other) and GetTag(other) or "unknown"
    print("Collided with: " .. tag)
end
IDE Type Definition Reference (api_stub.lua)RayneEngine provides full Language Server Protocol (LSP) type definitions:Lua---@meta

---@class Entity : integer

---@class Transform
---@field x number
---@field y number
---@field rotation number
---@field scaleX number
---@field scaleY number
---@field worldX number
---@field worldY number
---@field worldRotation number
---@field worldScaleX number
---@field worldScaleY number
Transform = {}

---@class Velocity
---@field dx number
---@field dy number
Velocity = {}

---@class Template
---@field path string
---@field Instantiate fun(self: Template, x: number, y: number, parent?: Entity): Entity

---@class Image
---@field __type string
---@field path string

---@class Vec2
---@field __type string
---@field x number
---@field y number

---@class Color
---@field __type string
---@field r integer
---@field g integer
---@field b integer

---@class EntityRef
---@field __type string
---@field name string

---@enum BodyType
BodyType = {
    Dynamic = 0,
    Kinematic = 1,
    Static = 2,
}

---@enum ColliderShape
ColliderShape = {
    Box = 0,
    Circle = 1,
}

---@class Rigidbody2D
---@field bodyType BodyType|integer
---@field mass number
---@field gravityScale number
---@field restitution number
---@field drag number
---@field freezeRotation boolean
Rigidbody2D = {}

---@class RaycastResult
---@field hit boolean
---@field entity Entity
---@field pointX number
---@field pointY number
---@field normalX number
---@field normalY number
---@field distance number
RaycastResult = {}
