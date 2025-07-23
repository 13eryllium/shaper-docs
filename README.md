# Shaper Documentation

> **Guide for creating objects in Shaper**

## Overview

`code.meow` contains the logic for your attack patterns and object behaviors. This file defines how your objects function.

## Available Events

Shaper allows you to access the following GM events:

| Event | Description |
|-------|-------------|
| `create` | Triggered once when the object is first instantiated |
| `clean_up` | Called when the object is being destroyed |
| `begin_step` | Executed at the start of each game step |
| `step` | Main logic executed during each game step |
| `end_step` | Executed at the end of each game step |
| `draw` | Handles rendering and visual updates |
| `draw_end` | Final drawing operations (goes above draw) |

## Event Implementation

Define events in your **code.meow** file using the following syntax:

### Basic Event Structure
```javascript
create = fun() {
  // Initialization logic here
}

step = fun() {
  // Main game logic
}

draw = fun() {
  // Sprite rendering
}
```

These three events are likely to be used in every one of your attacks.

---

## Functions

These are all the scripts you have access to. Most of these have their documentation in the GameMaker manual (they'll be linked or documented if they don't).

### External Libraries

#### **ALL** [**Icoso**](https://github.com/13eryllium/gml-scripts?tab=readme-ov-file#icoso_parseexpression) **functions**
*Read the documentation there.*

#### [**twerp()**](https://pixelatedpope.itch.io/twerp) & its in-house `twerp_ext()`
Tweening/easing functions for smooth animations.

### Drawing Functions

#### `draw_self()`
Draws the object's current sprite at its position.

#### `draw_sprite_ext()`
Enhanced sprite drawing with transformation options.

#### [`draw_sprite_anchored_ext()`](https://github.com/13eryllium/gml-scripts?tab=readme-ov-file#draw_sprite_anchored_ext)
Draws sprites with custom anchor points.

#### `draw_nexa_ext(x, y, text, c1, c2, c3, c4, xscale, yscale, angle, alpha, weight, halign, valign)`
Advanced text rendering with full transformation and color gradient support.

| Parameter | Description |
|-----------|-------------|
| `x` | The x coordinate of the drawn string |
| `y` | The y coordinate of the drawn string |
| `text` | The string to draw |
| `c1` | The colour for the top-left of the drawn text |
| `c2` | The colour for the top-right of the drawn text |
| `c3` | The colour for the bottom-right of the drawn text |
| `c4` | The colour for the bottom-left of the drawn text |
| `xscale` | The horizontal scale |
| `yscale` | The vertical scale |
| `angle` | The angle of the text |
| `alpha` | The alpha for the text |
| `weight` | The font weight of the text |
| `halign` | The horizontal alignment |
| `valign` | The vertical alignment |

#### `draw_nexa(x, y, text, c1, c2, xscale, yscale, weight, halign, valign)`
Simplified text rendering with vertical gradient support.

| Parameter | Description |
|-----------|-------------|
| `x` | The x coordinate of the drawn string |
| `y` | The y coordinate of the drawn string |
| `text` | The string to draw |
| `c1` | The colour for the top of the drawn text |
| `c2` | The colour for the bottom of the drawn text |
| `xscale` | The horizontal scale |
| `yscale` | The vertical scale |
| `weight` | The font weight of the text |
| `halign` | The horizontal alignment |
| `valign` | The vertical alignment |

### Random Functions

#### `random()`
Returns a random real number (equivalent to random_range).

#### `irandom()`
Returns a random integer (equivalent to irandom_range).

#### `choose()`
Randomly selects one value from the provided arguments.

### Object Control Functions

#### `kill()`
Destroys the object (equivalent to instance_destroy).

#### `offscreen(sprite)`
Checks if a sprite is outside the visible screen area.

| Parameter | Description |
|-----------|-------------|
| `sprite` | The sprite to check if is offscreen |

### Color Functions

#### `merge_color()`
Blends two colors together with a specified ratio.

### Math & Utility Functions

#### Trigonometry
- `dcos()` - Cosine using degrees
- `dsin()` - Sine using degrees
- `sin()` - Sine using radians
- `cos()` - Cosine using radians
- `deg_to_rad()` - Convert degrees to radians
- `rad_to_deg()` - Convert radians to degrees

#### Vector Math
- `lengthdir_x()` - Get X component of a vector from length and direction
- `lengthdir_y()` - Get Y component of a vector from length and direction
- `point_distance()` - Calculate distance between two points
- `point_direction()` - Calculate angle between two points
- `distance_to_object()` - Calculate distance to another object
- `angle_difference()` - Calculate the difference between two angles

#### General Math
- `lerp()` - Linear interpolation between two values
- `clamp()` - Constrain a value between minimum and maximum
- `min()` - Return the smallest value
- `max()` - Return the largest value

#### Collision Detection
- `point_in_rectangle()` - Check if a point is within a rectangular area

#### Array Functions
- `array_length()` - Get the length of an array
- `array_push()` - Add an element to the end of an array

#### Debug Functions
- `show_message()` - Display a debug message

---

## Constants

### Room Dimensions
- `room_width` - The width of the current room
- `room_height` - The height of the current room

### Text Alignment
#### Horizontal Alignment
- `fa_left` - Align text to the left
- `fa_center` - Center text horizontally
- `fa_right` - Align text to the right

#### Vertical Alignment
- `fa_top` - Align text to the top
- `fa_middle` - Center text vertically
- `fa_bottom` - Align text to the bottom

### Game-Specific Constants
- `level_color` - The current level's theme color
- `dt` (delta_time) - Time elapsed since the last frame (delta_time / 1000)

### Color Constants

- `c_jpink` - JSaB enemy shade of pink (#FF2070)

#### Basic Colors
- `c_white` - White color
- `c_black` - Black color
- `c_red` - Red color
- `c_green` - Green color
- `c_blue` - Blue color
- `c_yellow` - Yellow color
- `c_cyan` - Cyan color
- `c_magenta` - Magenta color

#### Gray Variants
- `c_gray` - Standard gray
- `c_ltgray` - Light gray
- `c_dkgray` - Dark gray
- `c_silver` - Silver color

#### Extended Colors
- `c_orange` - Orange color
- `c_purple` - Purple color
- `c_lime` - Lime green
- `c_pink` - Pink color
- `c_brown` - Brown color
- `c_navy` - Navy blue
- `c_maroon` - Maroon color
- `c_olive` - Olive color
- `c_teal` - Teal color
- `c_gold` - Gold color
- `c_crimson` - Crimson color
- `c_forest` - Forest green
- `c_royal` - Royal blue
- `c_violet` - Violet color
- `c_coral` - Coral color
- `c_mint` - Mint color
- `c_peach` - Peach color
