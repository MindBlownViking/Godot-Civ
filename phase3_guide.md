# Phase 3: UI/UX and Advanced Tile Behavior (Topo/Memo)

This is a memo file for later stage implementation (Phase 3) and covers the recommended moment to implement these systems:

- **Fog of War system** (basic implementation):
  1. Add a `CanvasLayer` with a full-screen black `ColorRect` or `LightOccluder2D`.
  2. Track visibility per tile (visible, explored, hidden).
  3. Update visibility based on units’ sight range each turn.
  4. Tie fog logic to terrain occlusion/collision where terrain should block vision (mountain, forest, etc.), so line-of-sight feels like classic Civ.
  5. For early prototyping you can use simple tile-based rules (e.g., single-tile vision mask) and refine later with true raycasts or occluders.
- **Fog of War enhancements** (refine from simple tile visibility to proper vision + occlusion) should happen in Phase 3.
- **Terrain occlusion/collision layers**: add when player/unit pathfinding and movement rules are stable.
  - mark mountains/water as blocked for movement and vision.
  - use RayCast2D/LightOccluder2D or tilemap-based LOS with occupancy.
- **Tile collision shape assignment** (mountain, water, etc.) belongs here as part of advanced gameplay polish.

Why Phase 3:
- Phase 2 focuses on map generation, core tiles, and initial procedural terrain.
- Phase 3 is the right time to polish interactions with the map, including vision, exploration, and path blockage.

Refer back to `phase2_guide.md` for core generation steps and move to Phase 3 once core gameplay is functionally working.
