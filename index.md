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
- **Project Hub** first-run setup modal for configuring project settings and standalone target specs
- Native **Visual Level Editor** with live property inspection, Inspector Lock, smooth scrolling, grid snapping, and undo/redo
- **Script Export Variables** supporting `Image`, `Vec2`, `Color`, `Entity`, and `Template` with live thumbnail previews and color picker
- Dedicated **UI Editor** for designing game HUDs visually on a fixed 1920×1080 canvas
- **In-Editor Console Panel** with live stdout/stderr capture, scrollable log, and command input
- Embedded **Lua 5.4 scripting environment** via `sol2` with lifecycle events and hot-reload
- Multi-channel **Audio Engine** and hardware input polling
- Complete **JSON Scene & UI Serialization** and project auto-saving
- **UI Manager** for runtime Text, Panel, and Button elements controlled from Lua
- **Standalone Game Export** creating an optimized `RayneGame` executable using your project settings

---

## Architecture

The engine architecture is structured into decoupled modules under `src/Core/`:
