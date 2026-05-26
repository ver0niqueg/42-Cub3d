# CUB3D 🎮

CUB3D is a raycasting engine project that creates a 3D first-person perspective view of a maze using ray casting techniques. It demonstrates fundamental graphics programming concepts including raycasting, texture mapping, collision detection, and real-time rendering. The goal is to build a functional 3D game engine capable of rendering immersive maze environments.

## Table of contents

- [Highlights](#highlights)
- [Functions](#functions)
- [Definitions](#definitions)
- [Installation](#installation)
- [Usage](#usage)
- [Controls](#controls)
- [Map Format](#map-format)
- [Testing](#testing)

## Highlights

- Implement raycasting engine to render 3D perspective from 2D map
- Support texture mapping for walls, floors, and ceilings
- Handle player movement and camera rotation in real-time
- Detect collisions with walls and map boundaries
- Parse custom `.cub` map files with validation
- Support both mandatory and bonus features (minimap, mouse control)
- Render RGB-textured walls with proper shading
- Handle edge cases: invalid maps, missing textures, impossible scenarios
- Implement efficient ray-wall intersection calculations

## Definitions

| Term | Description |
|------|-------------|
| Raycasting | Technique that casts rays from camera through each pixel to determine what's visible |
| Texture | Image file (.xpm) applied to surfaces for visual detail |
| Wall | Solid barrier in the map that blocks movement (represented by '1') |
| Floor | Walkable space (represented by '0' or player spawn) |
| Ceiling | The top surface parallel to the floor |
| Player | Controlled character in the maze (represented by N/S/E/W) |
| FOV | Field of View - the angular range visible from player position |
| RGB Color | Color format with Red, Green, Blue components (0-255 each) |
| Sprite | 2D image element (used in bonus features like minimap) |
| Collision | Detection when player intersects with walls or boundaries |
| Minimap | Bonus feature showing top-down view of the maze |
| Pitch | Vertical camera angle (looking up/down) |

## Functions

### Core Functions

**main**: Validates arguments and initializes the game engine

**parse_map**: Reads and validates `.cub` file format

**parse_textures**: Loads and validates texture files (.xpm)

**parse_colors**: Parses RGB values for floor and ceiling

**init_game**: Initializes game state, player position, and rendering

**raycasting_render**: Main raycasting loop for each frame

**cast_ray**: Casts individual ray from camera to find wall intersection

**draw_column**: Renders vertical line based on wall distance

**render_texture**: Applies texture mapping to rendered walls

**handle_keys**: Processes keyboard input for movement and rotation

**handle_mouse**: Bonus feature - mouse look controls

**init_minimap**: Bonus feature - initializes minimap rendering

**draw_minimap**: Bonus feature - renders top-down map view

**validate_map**: Ensures map has valid structure (surrounded by walls)

**free_resources**: Deallocates memory and destroys windows

## Installation

Clone the repository:
```bash
git clone git@github.com:ver0niqueg/42-Cub3d.git
cd 42-Cub3d
```

Compile mandatory version:
```bash
make
```

Compile bonus version (with minimap and mouse):
```bash
make bonus
```

Clean object files:
```bash
make clean
```

Remove all generated files:
```bash
make fclean
```

Recompile:
```bash
make re
```

## Usage

Run the program with a map file:
```bash
./cub3d <map_file.cub>
```

Or for bonus version:
```bash
./cub3d_bonus <map_file.cub>
```

### Parameters

- `map_file.cub`: Path to a valid CUB3D map file (must exist and be valid)

### Examples

```bash
# Simple maze
./cub3d maps/valid_maps/map1.cub

# Bonus version with minimap
./cub3d_bonus maps/valid_maps/map1.cub

# Test with different maps
./cub3d maps/valid_maps/map2.cub
./cub3d maps/valid_maps/map3.cub
```

## Controls

### Mandatory Version

| Key | Action |
|-----|--------|
| `W` | Move forward |
| `A` | Strafe left |
| `S` | Move backward |
| `D` | Strafe right |
| `← →` | Rotate camera left/right |
| `ESC` | Exit program |
| Click X | Exit program |

### Bonus Version (Additional Controls)

| Key | Action |
|-----|--------|
| `M` | Toggle minimap display |
| `Mouse Movement` | Look around (when enabled) |
| All mandatory controls | Same as above |

## Map Format

### File Structure

A valid `.cub` file must follow this format:

```
NO <path_to_north_texture>
SO <path_to_south_texture>
WE <path_to_west_texture>
EA <path_to_east_texture>

F <red>,<green>,<blue>
C <red>,<green>,<blue>

        1111111111111111111111111
        1000000000110000000000001
        1011000001110000000000001
        1001000000000000000000001
111111111011000001110000000000001
100000000011000001110111111111111
11110111111111011100000010001
11110111111111011101010010001
11000000110101011100000010001
10000000000000001100000010001
10000000000000001101010010001
11000001110101011111011110N0111
11110111 1110101 101111010001
11111111 1111111 111111111111
```

### Map Requirements

1. **Textures**: Must specify all 4 directions (NO, SO, WE, EA)
   - Paths must point to valid `.xpm` files
   - Files must be readable and non-empty

2. **Colors**: Floor (F) and Ceiling (C) colors required
   - Format: `R,G,B` (each 0-255)
   - No spaces around commas

3. **Map Layout**:
   - Must be surrounded by walls (1s)
   - Player spawn (N/S/E/W) must exist exactly once
   - Player cannot spawn on map edges
   - No holes in walls (isolated 0s surrounded by 1s)
   - Only valid characters: 0, 1, N, S, E, W, spaces

### Example Valid Map

```
NO textures/north.xpm
SO textures/south.xpm
WE textures/west.xpm
EA textures/east.xpm

F 220,100,0
C 225,30,0

111111
100001
101101
1N0001
111111
```

## Testing

### What to look for

✅ **No distortion**: Walls render without warping or artifacts  
✅ **Smooth movement**: Fluid player motion in all directions  
✅ **Proper texturing**: Textures align correctly on walls  
✅ **Color consistency**: Floor and ceiling use correct RGB colors  
✅ **Collision detection**: Player can't walk through walls  
✅ **No memory leaks**: Clean exit and resource deallocation  
✅ **Valid map parsing**: Rejects invalid maps gracefully  
✅ **Bonus features work**: Minimap displays accurately, mouse control responsive  

### Test scenarios

**Basic rendering**:
```bash
./cub3d maps/valid_maps/map1.cub
```
Expected: 3D maze renders correctly, movement works smoothly.

**Invalid map handling**:
```bash
./cub3d maps/error_map/map_missing.cub
```
Expected: Program exits with appropriate error message.

**Texture validation**:
```bash
./cub3d maps/error_map/textures_missing.cub
```
Expected: Program detects missing texture and fails gracefully.

**Player spawn positions**:
```bash
./cub3d maps/valid_maps/map2.cub
./cub3d maps/valid_maps/map3.cub
```
Expected: Player starts at correct position facing correct direction.

**Bonus features**:
```bash
./cub3d_bonus maps/valid_maps/map1.cub
```
Expected: Minimap displays in corner, mouse look controls work smoothly.

## Game Status Messages

The program outputs debug information:

```
Map file loaded successfully
All textures loaded
Game initialized
```

### Error Messages

| Error | Cause |
|-------|-------|
| `Error: Invalid map format` | Map file syntax is incorrect |
| `Error: Missing texture` | A required texture file is missing |
| `Error: Invalid RGB color` | Color values outside 0-255 range |
| `Error: Player not found` | No player spawn in map |
| `Error: Player on edge` | Player spawns on map boundary |
| `Error: Unclosed map` | Map not surrounded by walls |

## Key Concepts

### Raycasting Algorithm
- Cast rays from player position through each screen pixel
- Calculate intersection with nearest wall
- Determine wall distance and apply vertical distortion correction
- Apply texture mapping based on hit coordinates

### Texture Mapping
- Load XPM image files into memory
- Map 2D pixel coordinates onto 3D wall surfaces
- Apply proper scaling based on distance
- Handle texture coordinate wrapping

### Collision Detection
- Check player position against map grid
- Prevent movement into walls or boundaries
- Smooth sliding along walls

### Game Loop
- Parse input from keyboard/mouse
- Update player position and camera angle
- Perform raycasting calculations
- Render frame using miniLibX
- Repeat at screen refresh rate

### Bonus Features
- **Minimap**: Real-time top-down view with player indicator
- **Mouse Control**: Look around by moving mouse
- Additional visual enhancements

---

vgalmich & vihane
