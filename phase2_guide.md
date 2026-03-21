# Phase 2: Core Gameplay Systems Step-by-Step Guide

This guide walks you through implementing the core gameplay systems for a Civilization-style game in Godot. It focuses on essential systems in the order they typically need to be built and tested.

---
## 1. Map and World Generation

A tile-based map is the foundation of a Civ-like game.

### 1.1 Create a Tile Map Scene
1. In Godot, create a new `Node2D` scene named `MapRoot`.
2. Add a `TileMapLayer` node as a child.
3. Create a new `TileSet` resource and assign it to the TileMapLayer’s `tile_set` property.
   - In the Inspector, click the dropdown next to `Tile Set` and select "New TileSet".
   - Double-click it to open the TileSet editor.
   - Use an image editor (e.g., GIMP, Krita) to create small square images (32x32 pixels) filled with solid colors.
   - Add placeholder tiles to the TileSet, such as:
     - Grass (#228B22)
     - Water (#0000FF)
     - Mountain (#808080)
     - Plains (#FFFF00)
     - Desert (#FFA500)
     - Forest (#006400)
     - Hills (#8B4513)
     - Tundra (#ADD8E6)
     - Snow (#FFFFFF)
     - Ocean (#000080)
     - Coast (#87CEEB)
     - Save as PNGs in the assets/ folder (e.g., `grass.png`, `water.png`).
     - Import PNGs into Godot's FileSystem dock.
   - In TileSet editor, add each as a texture and define tiles:
     1. Click `Add Texture` (or the `+` icon) and select one placeholder PNG.
     2. Choose `Atlas` (not `Scenes Collection`) for static tile images.
     3. For single-tile PNGs, click the three dots, select `Create Tiles`, and confirm the non-transparent texture region (usually full 32x32 solid color).
     4. For atlas images (grids), set tile region to 32x32 and click `Create Atlas` (or `Create Tile`).
     5. Name the tile (e.g., Grass, Water, Mountain) for clarity.
     6. Repeat for each placeholder PNG.
     7. Leave occlusion/collision setup for Phase 3 (optionally add in Phase 3 when movement/blocking logic is complete).
   - Save the TileSet (e.g., as `res://tileset.tres`).
4. Save the scene to `scenes/map.tscn`.

### 1.2 Implement the Grid System
1. In `scripts/`, create a new script `map_generator.gd`.
2. Attach the script to `MapRoot` and add the following responsibilities:
   - Define map size (`width`, `height`).
   - Generate a 2D array to store tile types.
   - Place tiles on the TileMapLayer using `set_cell()`.

### 1.3 Procedural Terrain Generation
1. Implement a simple noise-based generator:
   - Use `OpenSimplexNoise` to generate height values.
   - Map height ranges to terrain types (water, grass, forest, mountain).
2. Optionally add a second noise layer for features (forests, hills).

### 1.4 Resource Placement
1. Define resource types in an enum (iron, horses, food, etc.).
2. Place resources on eligible tiles during generation.
3. Store resources in a parallel 2D array or a `ResourceCell` struct.

---
## 2. Basic Resource Management

Manage the core resources players build and spend each turn.

### 2.1 Define Core Resources
1. Create a `Resources.gd` script with a resource enum:
   - `Food`, `Production`, `Gold`, `Science`, `Culture`
2. Create a `ResourcePool` class (or `ResourceManager`) that tracks amounts.

### 2.2 Resource Accumulation and Consumption
1. Each city should have a `city_resources` instance.
2. At the end of each player turn, calculate yields from tiles and apply them.
3. Deduct upkeep costs (unit maintenance, building maintenance).

### 2.3 Terrain and Improvement Yields
1. Define yield values for each terrain type.
2. Add a system for tile improvements (farms, mines, roads).
3. Calculate total yields for a city based on its worked tiles.

---
## 3. City Management

Cities are the core of player economy, production, and growth.

### 3.1 City Foundation & Placement
1. Create a `City` scene with a root node (`Node2D` or `Area2D`) and a `Sprite`.
2. Add a `City.gd` script to handle city state (population, production, etc.).
3. When a Settler unit performs its “found city” action, instantiate the city at that tile.

### 3.2 City Growth and Population
1. Track city `population` as an integer.
2. Each turn, increase population based on food surplus.
3. Implement growth thresholds (e.g., each 10 food increases population).

### 3.3 Production Queue
1. Add a `ProductionItem` resource type (unit or building).
2. Track a queue in the city (array of production items).
3. Each turn, apply production to the current item and complete it when production reaches cost.

### 3.4 City Improvements and Specialization
1. Define improvement types (farm, mine, road, etc.) and their yield bonuses.
2. Allow players to build improvements on tiles (e.g., click tile, choose improvement).
3. City specialization can be modelled as simple yield multipliers (e.g., +20% science for research focus).

---
## 4. Unit System

Units move on the map, fight, and build.

### 4.1 Unit Types
1. Create a `Unit` base scene (`Node2D` or `CharacterBody2D`).
2. Define subtypes (Settler, Warrior, Archer) via scenes or exported enums.
3. Give each unit stats: movement points, strength, range, cost.

### 4.2 Production in Cities
1. Add unit production items to the city’s production queue.
2. When a unit finishes, instantiate the unit scene on a tile adjacent to the city.

### 4.3 Movement and Pathfinding
1. Use `Navigation2D` or A* to calculate paths across the tile grid.
2. Each unit should have `movement_points` and spend them each turn.
3. Handle turn-based movement: a unit can only move once per turn (or until it uses points).

### 4.4 Combat Mechanics
1. Define combat resolution (attack vs defense, terrain modifiers).
2. When one unit attacks another, calculate damage and adjust HP.
3. Implement ranged attacks (e.g., archers can hit units without moving).

### 4.5 Unit Promotions
1. Track experience points per unit.
2. When XP reaches the threshold, present promotion choices.
3. Apply promotion bonuses (e.g., +1 melee strength, +1 movement).

---
## 5. Technology and Research

A tech tree unlocks new units, buildings, and improvements.

### 5.1 Tech Tree Structure
1. Define tech nodes with prerequisites in a JSON or resource file.
2. Build a `TechTree` class that tracks unlocked techs and available techs.

### 5.2 Research Progress
1. Each turn, apply research points from cities to the currently selected tech.
2. When progress meets the required cost, unlock the tech.

### 5.3 Unlocks and Era Progression
1. When a tech is unlocked, enable new units/buildings/improvements.
2. Track eras (Ancient, Classical, etc.) based on tech progress.

---
## 6. Victory Conditions

Define how the game ends.

### 6.1 Domination Victory
1. Track capitals for each civilization.
2. If a player owns all capitals, trigger domination victory.

### 6.2 Science Victory
1. Define a “space project” buildable in a city.
2. When completed, trigger science victory.

### 6.3 Culture Victory
1. Track culture points per player.
2. When a player reaches a threshold, trigger culture victory.

### 6.4 Score and Time Victories
1. Calculate a score each turn based on cities, tech, wonders, etc.
2. If the turn limit is reached, end the game and compare scores.

---
## Next Steps

After building these core systems, you’ll be ready to move into Phase 3 (UI/UX) and Phase 4 (Diplomacy/AI). For now, make sure each system is playable and stable before layering on additional complexity.
