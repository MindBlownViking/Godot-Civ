# Phase 1: Project Foundation Step-by-Step Guide

This guide provides detailed instructions for completing Phase 1 of the Civilization Game Development Checklist in Godot.

## 1. Initialize Godot Project with Appropriate Settings (2D, Turn-Based)

Your project is already initialized (you have `project.godot`), but let's ensure it's configured correctly for a 2D turn-based strategy game.

### Steps:
1. **Open Godot Editor**: Launch Godot and open your project (`/home/deck/new-game-project-ia-help`).

2. **Verify Project Settings**:
   - Go to `Project` > `Project Settings` in the top menu.
   - In the `General` tab, under `Display` > `Window`:
	 - Set `Mode` to `canvas_items` (for proper 2D scaling in Godot 4).
	 - Adjust `Size` to your desired resolution (e.g., 1920x1080 for fullscreen).
	 - Set `Stretch Mode` to `canvas_items` for proper scaling.

3. **Configure for Turn-Based Gameplay**:
   - In `Project Settings`, go to `General` > `Application` > `Run`:
	 - Set `Main Scene` to a placeholder scene (you'll create this later).
   - For turn-based mechanics, you'll implement turn logic in scripts later, but ensure the project is set to run at a consistent frame rate.

4. **Save Settings**: Click `Close` to save changes.

## 2. Set Up Version Control (Git Repository)

Version control is essential for tracking changes and collaborating.

### Steps:
1. **Open Terminal**: In your project directory (`/home/deck/new-game-project-ia-help`).

2. **Initialize Git Repository**:
   ```
   git init
   ```

3. **Create .gitignore File**:
   - Create a new file named `.gitignore` in the project root.
   - Add the following content (standard for Godot projects):
	 ```
	 # Godot-specific ignores
	 .import/
	 *.tmp
	 *.import
	 export_presets.cfg

	 # OS-specific ignores
	 .DS_Store
	 Thumbs.db

	 # Editor-specific
	 .vscode/
	 .idea/
	 *.swp
	 *.swo
	 ```

4. **Initial Commit**:
   ```
   git add .
   git commit -m "Initial project setup"
   ```

5. **Optional: Connect to Remote Repository** (if you have GitHub/GitLab):
   ```
   git remote add origin <your-repo-url>
   git push -u origin main
   ```

## 3. Create Folder Structure

Organize your project files for better maintainability.

### Steps:
1. **Using Terminal** (recommended for consistency):
   ```
   cd /home/deck/new-game-project-ia-help
   mkdir scenes scripts assets ui
   ```

2. **Verify Structure**: Your project should now have:
   ```
   project.godot
   checklist.md
   .gitignore
   scenes/
   scripts/
   assets/
   ui/
   ```

3. **Optional Subfolders**: You can create additional organization as needed:
   ```
   mkdir assets/textures assets/audio assets/fonts
   mkdir scenes/levels scenes/ui
   ```

## 4. Configure Project Settings (Input Actions, Physics, etc.)

Fine-tune Godot's settings for your game.

### Steps:
1. **Open Project Settings**: `Project` > `Project Settings`.

2. **Configure Input Actions** (for turn-based controls):
   - Go to `Input Map` tab.
   - Add the following actions (click `+` and enter the name, then assign keys):
     - `end_turn`: Assign `Enter` or `Space`
     - `select`: Assign `Mouse Left Click`
     - `cancel`: Assign `Escape`
     - `move_up`: Assign `W` or `Up Arrow`
     - `move_down`: Assign `S` or `Down Arrow`
     - `move_left`: Assign `A` or `Left Arrow`
     - `move_right`: Assign `D` or `Right Arrow`
     - `zoom_in`: Assign `Mouse Wheel Up`
     - `zoom_out`: Assign `Mouse Wheel Down`

3. **Physics Settings** (for 2D tile-based game):
   - Go to `Physics` > `2D`:
     - Since this is a turn-based strategy game, you may not need real-time physics. You can disable physics processing in scenes where not needed.
     - Set `Default Gravity` to `0` if no gravity-based mechanics.

4. **Rendering Settings**:
   - In Project Settings, go to `Rendering` > `2d`:
     - You may only see **Snap Transforms to Pixel**; this is normal in Godot 4. Enable it if you want pixel-aligned positioning (useful for pixel-art projects).
   - For anti-aliasing, go to `Rendering` > `Quality` > `Anti Aliasing`:
     - Set `MSAA` to `2x` or `4x` for smoother edges if needed.
   - If you need to change the renderer, go to `Rendering` > `Renderer` and select the preferred rendering backend.

5. **General Settings**:
   - Go to `General` > `Application`:
     - Set `Config/Name` to your game name (e.g., "Civilization Clone").
     - Set `Config/Description` to a brief description.

6. **Save and Test**: Click `Close`. Test that the project runs without errors by pressing `F5` or `Play` button.

## Next Steps

Once Phase 1 is complete, you can move on to Phase 2: Core Gameplay Systems. Start with implementing the tile-based grid system for your map. Remember to commit your changes regularly with Git.
