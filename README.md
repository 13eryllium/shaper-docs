# Shaper Documentation

> **Guide for creating objects in Shaper**

## Overview

Shaper uses two main files to define attack patterns and object behaviors, as well as a few extra files for :

- **`code.meow`** - Contains the logic for your attack patterns and object behaviors
- **`data.json`** - Defines attack metadata and spawn parameters

### Level Structure

Every level has the following:
- `level.json` - The level file, containing attacks and metadata information
- `song.ogg` - The song that plays throughout the level
- `attacks/` - A folder for all your level's attacks
- `sprites/` - A folder for all your level's sprites

### Attack Structure

Attacks are stored in: `%localappdata%/thirteenE/Shaper/tracks/your_level/attacks/`

Each attack folder must include:
- `data.json` - Attack configuration and parameters
- `code.meow` - Behavior logic and event handlers
- `attack_icon.png` - The timeline icon for the attack (optional)
- `sprites/` - A folder for creating attack specific sprites that are accessed with the `my_sprites` variable in the object's create function

### data.json Structure

The `data.json` file configures your attack with the following properties:

- `attackName` - Display name shown in the interface
- `attackInternal` - Internal identifier (usually matches folder name)
- `appearInPalette` - Boolean determining if the attack appears in the palette
- `attackParams` - Array of spawn parameters for the object

### Attack Parameters

Each parameter in the `attackParams` array contains:

- `paramType` - Parameter type: `string`, `dropdown`, `sprite`, or `boolean`
    Setting this can change your param's suggestions.
    Dropdown sets the suggestions to `paramDropdown`.
    Sprite sets the suggestions to a list of all the sprites in your level's `sprites/` folder.
    Boolean sets the suggestions to `true` or `false`
&nbsp;

- `paramName` - Display name for the parameter (currently unused)
- `paramDefault` - Default value (always stored as string)
- `paramInt` - Internal name used in `code.meow`
- `paramDropdown` - Array of dropdown options if `paramType` is `dropdown`, otherwise `-1`
- `paramSuggestions` - Array of suggestions that pop up like `paramDropdown` in the console (optional)
    To have your suggestions be the Twerp easing modes, use `EASINGS` (string) as your paramSuggestions.

### Using Parameters in Code

For numeric parameters (positions, dimensions, etc.), use `eval()` to convert string values:

```javascript
create = fun(spawn_params, param_map, my_sprites) {
  -- Use self to set object variables instead of global variables
  -- param_map is mostly used with instantiate(), otherwise you will never use it.
  -- my_sprites is how you access your attack specific sprites

  self.sprite = my_sprites.sprite_name
  self.x = eval(spawn_params.x)

  -- This evaluates the spawn_params.x string into a number
}
```

### Sprites
You can add a sprite in  `%localappdata%/thirteenE/Shaper/tracks/your_track/your_level/sprites/yourspritename.png`

You reference a sprite by doing `sprite(yourspritename)`

The editor will automatically convert sprites in the folder into usable sprites for dropdown, so if you have a sprite in your `spawn_params`, you could use `sprite(spawn_params.sprite)`

By default, you have `square` and `circle` (1024x1024 images of the named shapes).

## Available Events

Shaper allows you to access the following GM events:

| Event | Description |
|-------|-------------|
| `create` | Triggered once when the object is first instantiated |
| `step` | Main logic executed during each game step |
| `draw` | Handles rendering and visual updates |
| `draw_begin` | Same as draw, but draws before it |
| `draw_end` | Same as draw, but draws over the player |

## Event Implementation

Define events in your **code.meow** file using the following syntax:

### Basic Event Structure
```javascript
create = fun(spawn_params, param_map, my_sprites) {
  -- Initialization logic here
}

step = fun() {
  -- Main game logic
}

draw_begin = fun() {
  -- Sprite rendering before the draw event
}

draw = fun() {
  -- Sprite rendering
}

draw_end = fun() {
  -- Sprite rendering over the player.
}
```

---

## Functions

These are all the scripts you have access to. Most of these have their documentation in the GameMaker manual (they'll be linked or documented if they don't).

### External Libraries

#### `eval(expression)`

Parses and evaluates a math expression, including variable substitution.

| Parameter    | Description                                           |
| ------------ | ----------------------------------------------------- |
| `expression` | A string representing a numeric expression or formula |

#### [**twerp()**](https://pixelatedpope.itch.io/twerp) & its in-house `twerp_ext()`
Tweening/easing functions for smooth animations.

#### `twerp_ext(type, start_val, end_val, duration, twerp_mode, delay)`
Advanced tweening function with extended features for animations.

| Parameter | Description |
|-----------|-------------|
| `type` | The easing type (e.g., "Linear", "EaseOutQuad", "EaseInCubic", etc.) |
| `start_val` | Starting value of the animation |
| `end_val` | Ending value of the animation |
| `duration` | Duration of the animation in seconds |
| `twerp_mode` | Animation mode: "once", "loop", or "patrol" |
| `delay` | Delay before animation starts (optional, default: 0) |

| Easing Type | Description |
|-------------|-------------|
| `Linear` | Constant speed interpolation |
| **Back** |
| `EaseInBack` | Starts backward then accelerates forward |
| `EaseOutBack` | Overshoots then settles back |
| `EaseInOutBack` | Combines EaseInBack and EaseOutBack |
| `EaseInBackHard` | Intense backward start |
| `EaseOutBackHard` | Intense overshoot |
| `EaseInOutBackHard` | Combines hard back easing |
| `EaseInBackSoft` | Gentle backward start |
| `EaseOutBackSoft` | Gentle overshoot |
| `EaseInOutBackSoft` | Combines soft back easing |
| **Bounce** |
| `EaseInBounce` | Bouncing effect at start |
| `EaseOutBounce` | Bouncing effect at end |
| `EaseInOutBounce` | Combines EaseInBounce and EaseOutBounce |
| **Circle** |
| `EaseInCircle` | Circular acceleration |
| `EaseOutCircle` | Circular deceleration |
| `EaseInOutCircle` | Combines EaseInCircle and EaseOutCircle |
| **Cubic** |
| `EaseInCubic` | Cubic acceleration |
| `EaseOutCubic` | Cubic deceleration |
| `EaseInOutCubic` | Combines EaseInCubic and EaseOutCubic |
| **Elastic** |
| `EaseInElastic` | Elastic oscillation at start |
| `EaseOutElastic` | Elastic oscillation at end |
| `EaseInOutElastic` | Combines EaseInElastic and EaseOutElastic |
| **Jelly** |
| `EaseInJelly` | Jelly-like wobble at start |
| `EaseOutJelly` | Jelly-like wobble at end |
| `EaseInOutJelly` | Combines EaseInJelly and EaseOutJelly |
| **Expo** |
| `EaseInExpo` | Exponential acceleration |
| `EaseOutExpo` | Exponential deceleration |
| `EaseInOutExpo` | Combines EaseInExpo and EaseOutExpo |
| **Quad** |
| `EaseInQuad` | Quadratic acceleration |
| `EaseOutQuad` | Quadratic deceleration |
| `EaseInOutQuad` | Combines EaseInQuad and EaseOutQuad |
| **Quart** |
| `EaseInQuart` | Quartic acceleration |
| `EaseOutQuart` | Quartic deceleration |
| `EaseInOutQuart` | Combines EaseInQuart and EaseOutQuart |
| **Quint** |
| `EaseInQuint` | Quintic acceleration |
| `EaseOutQuint` | Quintic deceleration |
| `EaseInOutQuint` | Combines EaseInQuint and EaseOutQuint |
| **Sine** |
| `EaseInSine` | Sinusoidal acceleration |
| `EaseOutSine` | Sinusoidal deceleration |
| `EaseInOutSine` | Sinusoidal acceleration and deceleration |

#### `twerp_process(twerp_asset)`
Updates and returns the current value of the twerp asset.

| Parameter | Description |
|-----------|-------------|
| `twerp_asset` | The twerp asset to process |

#### `twerp_get(twerp_asset)`
Returns the current value of the twerp asset.

| Parameter | Description |
|-----------|-------------|
| `twerp_asset` | The twerp asset to return |

#### `twerp_percent(twerp_asset)`
Returns the current progress percentage (0.0 to 1.0) of the animation.

| Parameter | Description |
|-----------|-------------|
| `twerp_asset` | The twerp asset to check |

#### `twerp_reset(twerp_asset)`
Resets the twerp asset to its initial state.

| Parameter | Description |
|-----------|-------------|
| `twerp_asset` | The twerp asset to reset |

#### `twerp_has_finished(twerp_asset)`
Checks if the animation has completed.

| Parameter | Description |
|-----------|-------------|
| `twerp_asset` | The twerp asset to check |

#### `twerp_has_finished_delay(twerp_asset)`
Checks if the delay period has finished and the animation is ready to start.

| Parameter | Description |
|-----------|-------------|
| `twerp_asset` | The twerp asset to check |

#### `twerp_chain_finished(twerp_asset)`
Checks if both the main animation and any chained animations have completed.

| Parameter | Description |
|-----------|-------------|
| `twerp_asset` | The twerp asset to check |

#### `twerp_chain(parent_twerp, type, start_val, end_val, duration, twerp_mode, delay)`
Creates a chained animation that starts when the parent animation finishes.

| Parameter | Description |
|-----------|-------------|
| `parent_twerp` | The parent twerp asset to chain from |
| `type` | The easing type for the chained animation |
| `start_val` | Starting value of the chained animation |
| `end_val` | Ending value of the chained animation |
| `duration` | Duration of the chained animation in seconds |
| `twerp_mode` | Animation mode: "once", "loop", or "patrol" |
| `delay` | Delay before chained animation starts (optional, default: 0) |

#### `twerp_chain_duplicate(parent_twerp, type, start_val, end_val, duration, twerp_mode, delay)`
Creates an independent twerp that waits for the parent to finish before starting.

| Parameter | Description |
|-----------|-------------|
| `parent_twerp` | The parent twerp asset to wait for |
| `type` | The easing type for the new animation |
| `start_val` | Starting value of the new animation |
| `end_val` | Ending value of the new animation |
| `duration` | Duration of the new animation in seconds |
| `twerp_mode` | Animation mode: "once", "loop", or "patrol" |
| `delay` | Delay before new animation starts (optional, default: 0) |

#### Collision Detection
#### `point_in_rectangle()`
Check if a point is within a rectangular area

#### `get_player(player)`
Retrieves player data based on the specified parameter.

| Parameter | Description |
|-----------|-------------|
| `player` | Can be "nearest" (closest player), "all" (array of all players), or a number (0-3) for specific player |

Returns a struct (or array of structs for "all") with the following properties:

| Property | Description |
|----------|-------------|
| `x` | Player's x coordinate |
| `y` | Player's y coordinate |
| `is_moving` | Boolean indicating if the player is currently moving |
| `dashing` | Boolean indicating if the player is actively dashing |
| `color` | Player's main color |
| `shape` | Player's shape index |
| `rect` | Player's collision rectangle array |
| `horizontal` | Player's horizontal input/direction (-1, 1) |
| `vertical` | Player's vertical input/direction (-1, 1) |
| `dead` | Boolean indicating if the player is dead |
| `body` | Reference to the actual player object instance |
| `number` | The player's index in game (0-3) |

#### `get_player_rects()`
Returns an array of all player collision rectangles.

#### `players_in_collison(collision, [not?])`
Takes a collision array and returns corresponding player data.

| Parameter | Description |
|-----------|-------------|
| `collision` | Array of boolean collision results |
| `not?` | Detect players outside of points instead of inside (defaults to `false`) |

Returns an array where `true` collision indices contain player structs, `false` indices contain `undefined`.

#### `hit_players_in_points(points, [damage], [not?])`
Checks collision between players and collision points, causing damage to the players inside the points.

| Parameter | Description |
|-----------|-------------|
| `points` | Collision points struct created by `create_collision_points()` |
| `damage` | Damage dealt (defaults to `1`) |
| `not?` | Hit players outside of points instead of inside (defaults to `false`) |

#### `hit_player(x, y, player, [damage], [ignore?])`
Hits a specified player regardless of if they have invincibility frames / are dashing.

| Parameter | Description |
|-----------|-------------|
| `x` | X position of the damage source for knockback |
| `y` | Y position of the damage source for knockback |
| `player` | The player to target |
| `damage` | Damage dealt (defaults to `1`) |
| `ignore?` | Whether or not invincibility frames / dashing are ignored (defaults to `true`) |

#### `create_collision_points(x1, y1, width, height, angle)`
Creates a rotated rectangle collision area with corner points.

| Parameter | Description |
|-----------|-------------|
| `x1` | Center x coordinate |
| `y1` | Center y coordinate |
| `width` | Rectangle width |
| `height` | Rectangle height |
| `angle` | Rotation angle in degrees |

Returns a struct with `corner1-4` arrays `[x, y]`, `center` array, `width`, `height`, and `angle`.

#### `create_collision_points_anchored(x1, y1, width, height, angle, xoff, yoff)`
Creates a rotated rectangle collision area with corner points, anchored at a certain point of the rectangle.

| Parameter | Description |
|-----------|-------------|
| `x1` | Center x coordinate |
| `y1` | Center y coordinate |
| `width` | Rectangle width |
| `height` | Rectangle height |
| `angle` | Rotation angle in degrees |
| `xoff` | The horizontal anchor point (percentage) of the rectangle (0 is left, 1 is right, 0.5 is center) |
| `yoff` | The vertical anchor point (percentage) of the rectangle (0 is top, 1 is bottom, 0.5 is center)|

Returns a struct with `corner1-4` arrays `[x, y]`, `center` array, `width`, `height`, and `angle`.

#### `check_collision_points(array_or_rect_array, collision_points)`
Tests collision between rectangles and rotated collision points.

| Parameter | Description |
|-----------|-------------|
| `array_or_rect_array` | Single rectangle array [x1, y1, x2, y2] or array of rectangles |
| `collision_points` | Collision points struct from `create_collision_points()` |

Returns boolean result for single rectangle or array of boolean results for multiple rectangles.

#### `check_single_rectangle_collision(rect, collision_points)`
Internal function that performs collision detection between a single rectangle and collision points.

| Parameter | Description |
|-----------|-------------|
| `rect` | Rectangle array [x1, y1, x2, y2] |
| `collision_points` | Collision points struct |

#### `debug_draw_collision_points(collision_points, color)`
Draws the collision area outline for debugging purposes.

| Parameter | Description |
|-----------|-------------|
| `collision_points` | Collision points struct to visualize |
| `color` | Color to draw the outline |


#### `point_clamp(points, position)`
Clamps an array position into a set of points.

| Parameter | Description |
|-----------|-------------|
| `points` | Collision points struct from `create_collision_points()` |
| `position` | X and Y array `[0, 0]` |

Returns an array of the clamped position `[0, 0]`.

#### `set_player_position(player, x, y)`
Sets the position of a player

| Parameter | Description |
|-----------|-------------|
| `player` | The player number (0-3) |
| `x` | X position in world |
| `y` | Y position in world |

#### `get_player_position(player)`
Gets the position of a player

| Parameter | Description |
|-----------|-------------|
| `player` | The player number (0-3) |

Returns an array of the given player's position `[0, 0]`.

### Drawing Functions

#### `sprite_get_width()`
Gets the width of a given sprite

#### `sprite_get_height()`
Gets the height of a given sprite

#### `draw_circle()`
Draws a circle

#### `draw_circle_color()`
Draws a gradient circle

#### `draw_rectangle()`
Draws a rectangle

#### `draw_rectangle_color()`
Draws a gradient rectangle

#### `draw_line()`
Draws a line

#### `draw_line_width()`
Draws a line with width

#### `draw_set_color()`
Sets the color for most drawing operations

#### `draw_get_color()`
Gets the color for most drawing operations

#### `draw_set_alpha()`
Sets the alpha for most drawing operations

#### `draw_get_alpha()`
Gets the alpha for most drawing operations


#### `draw_dashed_line(color)`
Sets the global color of the level (`level_color` constant).

| Parameter | Description |
|-----------|-------------|
| `color` | The target color for the level |


#### `draw_dashed_line(x1, y1, x2, y2, dash_length, gap_length, thickness, line_color, [alpha])`
Draws a dashed line

| Parameter | Description |
|-----------|-------------|
| `x1` | The line's starting x |
| `y1` | The line's starting y |
| `x2` | The line's ending x |
| `y2` | The line's ending y |
| `dash_length` | The length of the dashes |
| `gap_length` | The length of the gap between the dashes |
| `thickness` | The width of the line |
| `line_color` | The color of the line |
| `alpha` | The alpha of the line (default `1`) |

#### `draw_dashed_line_offset(x1, y1, x2, y2, dash_length, gap_length, thickness, line_color, offset, alpha)`
Draws a dashed line between two points with a configurable dash offset.

| Parameter | Description |
|-----------|-------------|
| `x1` | The line's starting x |
| `y1` | The line's starting y |
| `x2` | The line's ending x |
| `y2` | The line's ending y |
| `dash_length` | Length of each dash segment |
| `gap_length` | Length of the gap between dashes |
| `thickness` | Width of the line |
| `line_color` | Color of the dashed line |
| `offset` | Offset applied to the dash pattern along the line |
| `alpha` | Alpha transparency of the line |

#### `draw_self()`
Draws the object's current sprite at its position.

#### `draw_sprite_ext()`
Enhanced sprite drawing with transformation options.

#### [`draw_sprite_anchored_ext(sprite, subimage, x, y, xscale, yscale, angle, blend, alpha, xoff, yoff)`](https://github.com/13eryllium/gml-scripts?tab=readme-ov-file#draw_sprite_anchored_ext)
Draws sprites with custom anchor points.

#### `draw_sprite_fog(sprite, subimg, _x, _y, xscale, yscale, rotation, blend, fog_blend, alpha, fog_alpha)`
Draws sprites with an optional completely solid color overlay.


#### `draw_sprite_anchored_fog(sprite, subimg, x, y, xscale, yscale, rotation, blend, fog_blend, alpha, fog_alpha, xoff, yoff)`
Draws a sprite with a custom anchor point and an optional solid color overlay, combining the functionality of `draw_sprite_anchored_ext` and `draw_sprite_fog`.

| Parameter | Description |
|-----------|-------------|
| `sprite` | The sprite to draw |
| `subimg` | The frame of the sprite to draw |
| `x` | The x coordinate of the sprite |
| `y` | The y coordinate of the sprite |
| `xscale` | The horizontal scale of the sprite |
| `yscale` | The vertical scale of the sprite |
| `rotation` | The rotation angle of the sprite in degrees |
| `blend` | The tint color of the sprite |
| `fog_blend` | The color of the solid overlay |
| `alpha` | The alpha transparency of the sprite |
| `fog_alpha` | The alpha of the solid color overlay |
| `xoff` | The horizontal anchor offset in pixels |
| `yoff` | The vertical anchor offset in pixels |

| Parameter | Description |
|-----------|-------------|
| `sprite` | The sprite to draw |
| `subimage` | The frame of the image to draw |
| `x` | The x coordinate of the image |
| `y` | The y coordinate of the image |
| `xscale` | The width of the sprite |
| `yscale` | The height of the sprite |
| `rotation` | The angle of the sprite |
| `blend` | The tint of the sprite |
| `fog_blend` | The color of the solid overlay |
| `alpha` | The alpha of the image |
| `fog_alpha` | The alpha of the solid overlay |

#### `draw_sprite_cubed_ext(sprite, subimg, x, y, size, pitch, yaw, roll, color, alpha, shading)`
Draws a sprite as if it were painted onto all visible faces of a 3D cube, with full pitch, yaw, and roll rotation.

| Parameter | Description |
|-----------|-------------|
| `sprite` | The sprite to draw |
| `subimg` | The frame of the sprite to draw |
| `x` | The x coordinate of the cube's center |
| `y` | The y coordinate of the cube's center |
| `size` | Half-size (radius) of the cube in pixels |
| `pitch` | Rotation around the X axis in degrees |
| `yaw` | Rotation around the Y axis in degrees |
| `roll` | Rotation around the Z axis in degrees |
| `color` | The tint color applied to the sprite |
| `alpha` | The alpha transparency of the sprite |
| `shading` | Whether to apply shading across faces |

#### `draw_sprite_skew_ext(sprite, index, x, y, xscale, yscale, angle, angle_sin, tint, alpha, hskew, vskew)`
Draws a sprite with rotation, scaling, and horizontal/vertical skew applied.

| Parameter | Description |
|-----------|-------------|
| `sprite` | The sprite to draw |
| `index` | The sprite subimage index |
| `x` | X position to draw the sprite |
| `y` | Y position to draw the sprite |
| `xscale` | Horizontal scale of the sprite |
| `yscale` | Vertical scale of the sprite |
| `angle` | Rotation angle in degrees |
| `angle_sin` | Rotation angle in degrees (used to calculate sine internally) |
| `tint` | Color tint applied to the sprite |
| `alpha` | Alpha transparency of the sprite |
| `hskew` | Horizontal skew amount |
| `vskew` | Vertical skew amount |

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

### Camera Functions

#### `screen_bounds(padding)`
Returns a struct containing the limits of the camera with optional padding. (`left`, `right`, `up`, `down`)

| Parameter | Description |
|-----------|-------------|
| `padding` | The player number (0-3) |

#### `camera_set_pos(x, y)`
Sets the camera's position directly.

| Parameter | Description |
|-----------|-------------|
| `x` | Target x coordinate |
| `y` | Target y coordinate |

#### `camera_set_shake(x, y)`
Sets the camera's shake intensity.

| Parameter | Description |
|-----------|-------------|
| `x` | Horizontal shake intensity |
| `y` | Vertical shake intensity |

#### `camera_kick_pos_angle(angle, force, duration)`
Kicks the camera's position in a direction defined by an angle and force, then eases it back to its original position.

| Parameter | Description |
|-----------|-------------|
| `angle` | Angle of the kick in degrees |
| `force` | Distance of the kick |
| `duration` | How long the camera takes to ease back |

#### `camera_kick_pos_xy(xoff, yoff, duration)`
Kicks the camera's position by an x/y offset, then eases it back.

| Parameter | Description |
|-----------|-------------|
| `xoff` | Horizontal offset of the kick |
| `yoff` | Vertical offset of the kick |
| `duration` | How long the camera takes to ease back |

#### `camera_kick_angle(angle, duration)`
Kicks the camera's angle by the specified amount, then eases it back to its original angle.

| Parameter | Description |
|-----------|-------------|
| `angle` | Angle to kick by in degrees |
| `duration` | How long the camera takes to ease back |

#### `camera_set_angle(angle)`
Sets the camera's rotation directly.

| Parameter | Description |
|-----------|-------------|
| `angle` | Target angle in degrees |

#### `camera_kick_zoom(zoom, duration)`
Kicks the camera's zoom toward the specified level, then eases it back.

| Parameter | Description |
|-----------|-------------|
| `zoom` | Target zoom level for the kick |
| `duration` | How long the camera takes to ease back |

#### `camera_set_zoom(zoom)`
Sets the camera's zoom level directly.

| Parameter | Description |
|-----------|-------------|
| `zoom` | Target zoom level (`1` is default) |

#### `camera_clear_tweens()`
Removes all active camera tweens immediately.

#### `camera_reset()`
Resets the camera to its default state: centered position, no rotation, zoom of `1`, no shake, and all tweens and kick offsets cleared.

#### `camera_tween(tween_name, tween_type, tween_mode, duration, props, [add_to_raw])`
Creates a named tween that animates the camera toward target property values.

| Parameter | Description |
|-----------|-------------|
| `tween_name` | A string identifier for this tween (used to reference or destroy it later) |
| `tween_type` | The easing type (e.g. `"EaseOutQuad"`) |
| `tween_mode` | Animation mode: `"once"`, `"loop"`, or `"patrol"` |
| `duration` | Duration of the tween in seconds |
| `props` | A struct with any of the following optional fields: `x`, `y`, `angle`, `zoom`. Any omitted fields will stay at their current values |
| `add_to_raw` | If `true`, the tween's delta is added to `raw` values instead of replacing them on completion. Defaults to `false` |

#### `camera_tween_ext(tween_name, tween_type, tween_mode, duration, start_props, end_props, [add_to_raw])`
Like `camera_tween()`, but lets you explicitly define both the start and end values for each property.

| Parameter | Description |
|-----------|-------------|
| `tween_name` | A string identifier for this tween |
| `tween_type` | The easing type (e.g. `"EaseInCubic"`) |
| `tween_mode` | Animation mode: `"once"`, `"loop"`, or `"patrol"` |
| `duration` | Duration of the tween in seconds |
| `start_props` | A struct defining starting values for any of: `x`, `y`, `angle`, `zoom` |
| `end_props` | A struct defining ending values for the same properties |
| `add_to_raw` | If `true`, the tween's delta is added to `raw` values on completion. Defaults to `false` |

#### `camera_tween_destroy(tween_name)`
Removes a specific named tween, stopping it immediately without snapping to its end value.

| Parameter | Description |
|-----------|-------------|
| `tween_name` | The string identifier of the tween to remove |

#### `camera_get_raw_pos()`
Returns the camera's logical position, before shake and kick offsets are applied.

Returns a struct with the following properties:

| Property | Description |
|----------|-------------|
| `x` | Raw x position |
| `y` | Raw y position |
| `angle` | Raw angle |
| `zoom` | Raw zoom level |

#### `camera_get_real_pos()`
Returns the camera's actual final position, including all shake, kick, and tween offsets.

Returns a struct with the following properties:

| Property | Description |
|----------|-------------|
| `x` | Final x position |
| `y` | Final y position |
| `angle` | Final angle |
| `zoom` | Final zoom level |

#### `camera_apply_camera(view_camera)`
Applies the camera's current state to a GameMaker view camera, updating its position, size, and angle.

| Parameter | Description |
|-----------|-------------|
| `view_camera` | The view camera to apply the state to |

### Object Control Functions

#### `kill()`
Destroys the object (equivalent to instance_destroy).

#### `instantiate(object, params, param_map)`
Creates an object (equivalent to instance_create).

| Parameter | Description |
|-----------|-------------|
| `object` | The string of the object name to spawn (attack_internal) |
| `params` | The params (spawn_params) to instantiate the object with |
| `param_map` | A set of params that the child object can access (set in the parent) |

#### `offscreen(sprite)`
Checks if a sprite is outside the visible screen area.

| Parameter | Description |
|-----------|-------------|
| `sprite` | The sprite to check if is offscreen |

#### `sprite(sprite name)`
Gets a sprite to use in drawing.

| Parameter | Description |
|-----------|-------------|
| `sprite` | The sprite to get |

---

#### `apparition_ghost(position, duration, scale, additive)`

Creates a shrinking circle apparition used in bullet visuals that moves relative to the bullet with a speed multiplier.

| Parameter  | Description                               |
| ---------- | ----------------------------------------- |
| `position` | The position where the apparition appears |
| `duration` | How long the apparition lasts             |
| `scale`    | The size of the apparition                |
| `additive` | Multiple to apply to the movement speed   |

---

#### `apparition_stick(position, duration, scale)`

Creates a shrinking circle apparition used in bullet visuals that stays on top of the bullet.

| Parameter  | Description                               |
| ---------- | ----------------------------------------- |
| `position` | The position where the apparition appears |
| `duration` | How long the apparition lasts             |
| `scale`    | The size of the apparition                |

---

#### `trail_create(delay, sprite, lifetime, size, rot_speed, shake)`

Creates a trail effect for bullets.

| Parameter   | Description                     |
| ----------- | ------------------------------- |
| `delay`     | Time between trail segments     |
| `sprite`    | Sprite used for trail segments  |
| `lifetime`  | Duration of each trail segment  |
| `size`      | Size of each segment            |
| `rot_speed` | Rotation speed of segments      |
| `shake`     | Amount of jitter added to trail |

---

#### `create_angle_bullet_ext(_x, _y, angle, rot_speed, spd, size, curve, sprite, app_struct, trail_struct)`

Creates a bullet that moves based on angle and speed.

| Parameter      | Description                 |
| -------------- | --------------------------- |
| `_x`           | Initial x position          |
| `_y`           | Initial y position          |
| `angle`        | Initial movement angle      |
| `rot_speed`    | How fast the bullet rotates |
| `spd`          | Movement speed              |
| `size`         | Bullet size                 |
| `curve`        | Path curvature factor       |
| `sprite`       | Bullet sprite               |
| `app_struct`   | Optional apparition struct  |
| `trail_struct` | Optional trail struct       |

---

#### `create_angle_bullet(_x, _y, angle, rot_speed, spd, size, curve, sprite, app_scale, app_pos, app_duration)`

Creates an angle-based bullet with an automatic stick apparition.

| Parameter      | Description                 |
| -------------- | --------------------------- |
| `_x`           | Initial x position          |
| `_y`           | Initial y position          |
| `angle`        | Initial movement angle      |
| `rot_speed`    | How fast the bullet rotates |
| `spd`          | Movement speed              |
| `size`         | Bullet size                 |
| `curve`        | Path curvature factor       |
| `sprite`       | Bullet sprite               |
| `app_scale`    | Apparition size             |
| `app_pos`      | Apparition position         |
| `app_duration` | Apparition lifetime         |

---

#### `create_velocity_bullet_ext(_x, _y, xv, yv, rot_speed, grv, size, sprite, app_struct, trail_struct)`

Creates a bullet that moves with x/y velocity and gravity.

| Parameter      | Description                  |
| -------------- | ---------------------------- |
| `_x`           | Initial x position           |
| `_y`           | Initial y position           |
| `xv`           | Horizontal velocity          |
| `yv`           | Vertical velocity            |
| `rot_speed`    | How fast the bullet rotates  |
| `grv`          | Gravity affecting the bullet |
| `size`         | Bullet size                  |
| `sprite`       | Bullet sprite                |
| `app_struct`   | Optional apparition struct   |
| `trail_struct` | Optional trail struct        |

---

#### `create_velocity_bullet(_x, _y, xv, yv, rot_speed, grv, size, sprite, app_scale, app_pos, app_duration)`

Creates a velocity-based bullet with an automatic ghost apparition.

| Parameter      | Description                  |
| -------------- | ---------------------------- |
| `_x`           | Initial x position           |
| `_y`           | Initial y position           |
| `xv`           | Horizontal velocity          |
| `yv`           | Vertical velocity            |
| `rot_speed`    | How fast the bullet rotates  |
| `grv`          | Gravity affecting the bullet |
| `size`         | Bullet size                  |
| `sprite`       | Bullet sprite                |
| `app_scale`    | Apparition size              |
| `app_pos`      | Apparition position          |
| `app_duration` | Apparition lifetime          |

---

### Color Functions

#### `merge_color()`
Blends two colors together with a specified ratio.

### Math & Utility Functions

#### Trigonometry
- `dcos()` - Cosine using degrees
- `dsin()` - Sine using degrees
- `sin()` - Sine using radians
- `cos()` - Cosine using radians
- `degtorad()` - Convert degrees to radians
- `radtodeg()` - Convert radians to degrees

#### Vector Math
- `lengthdir_x()` - Get X component of a vector from length and direction
- `lengthdir_y()` - Get Y component of a vector from length and direction
- `point_distance()` - Calculate distance between two points
- `point_direction()` - Calculate angle between two points
- `distance_to_object()` - Calculate distance to another object
- `angle_difference()` - Calculate the difference between two angles

- `midpoint(x1, y1, x2, y2)` - Calculate the midpoint between two points

| Parameter     | Description |
|---------------|-------------|
| `x1` | Point 1 X |
| `y1` | Point 1 Y |
| `x2` | Point 2 X |
| `y2` | Point 2 Y |

Returns an array containing the midpoint `[x, y]`.

Here's a documentation block for those functions:

---

### HorriFi Effects

---

#### `horrifi_bloom_set(enabled, radius, intensity, threshold)`
Configures the bloom effect in one call.

| Parameter   | Type    | Max | Description                              |
|-------------|---------|-----|------------------------------------------|
| `enabled`   | boolean | —   | Enables or disables bloom                |
| `radius`    | number  | 32  | Spread radius of the bloom               |
| `intensity` | number  | 1   | Brightness of the bloom                  |
| `threshold` | number  | 1   | Luminance threshold before bloom applies |

---

#### `horrifi_chromaticab_set(enabled, strength)`
Configures the chromatic aberration effect in one call.

| Parameter  | Type    | Max | Description                          |
|------------|---------|-----|--------------------------------------|
| `enabled`  | boolean | —   | Enables or disables chromatic aberration |
| `strength` | number  | 1   | Intensity of the color fringing      |

---

#### `horrifi_scanlines_set(enabled, strength)`
Configures the scanlines effect in one call.

| Parameter  | Type    | Max | Description                      |
|------------|---------|-----|----------------------------------|
| `enabled`  | boolean | —   | Enables or disables scanlines    |
| `strength` | number  | 1   | Opacity of the scanline overlay  |

---

#### `horrifi_vhs_set(enabled, strength)`
Configures the VHS distortion effect in one call.

| Parameter  | Type    | Max | Description                        |
|------------|---------|-----|------------------------------------|
| `enabled`  | boolean | —   | Enables or disables VHS distortion |
| `strength` | number  | 1   | Intensity of the VHS warping       |

---

#### `horrifi_vignette_set(enabled, strength, intensity)`
Configures the vignette effect in one call.

| Parameter   | Type    | Max | Description                          |
|-------------|---------|-----|--------------------------------------|
| `enabled`   | boolean | —   | Enables or disables the vignette     |
| `strength`  | number  | 1   | Size of the vignette border          |
| `intensity` | number  | 1   | Darkness of the vignette edges       |

---

#### `horrifi_noise_set(enabled, strength)`
Configures the noise/grain effect in one call.

| Parameter  | Type    | Max | Description                    |
|------------|---------|-----|--------------------------------|
| `enabled`  | boolean | —   | Enables or disables noise      |
| `strength` | number  | 1   | Intensity of the film grain    |

### Effects

#### `effect_layer_set(slot, effect_name)`  
Sets a post-processing effect on the specified layer slot (1–3).

| Parameter     | Description |
|---------------|-------------|
| `slot`        | Effect layer slot (clamped from 1 to 3) |
| `effect_name` | The name of the effect to apply |

---

#### `effect_layer_set_silent(slot, effect_name)`  
Sets a post-processing effect on the specified slot with zero intensity.

| Parameter     | Description |
|---------------|-------------|
| `slot`        | Effect layer slot (clamped from 1 to 3) |
| `effect_name` | The name of the effect to prepare silently |

---

#### `effect_set(slot, parameter, value)`  
Updates a specific parameter of the effect on the given slot.

| Parameter   | Description |
|-------------|-------------|
| `slot`      | Effect layer slot (1–3) |
| `parameter` | Parameter to change (e.g., `intensity`, `color`) |
| `value`     | New value for the parameter (number, color, or array) |

---

#### `effect_layer_clear(slot)`  
Removes any post-processing effect from the specified slot.

| Parameter | Description |
|-----------|-------------|
| `slot`    | Effect layer slot (1–3) |

### Effect Table
| Effect Name       | Adjustable Parameters |
|-------------------|------------------------|
| `Desaturate`      | `intensity`           |
| `ColorTint`       | `color`, `tint`       |
| `ZoomBlur`        | `intensity`, `focus_radius`, `center`, `position` |
| `Pixelate`        | `cell_size`           |
| `GaussianBlur`    | `downsample_count`, `passes` |
| `WhiteNoise`      | `intensity`, `animation` |
| `Glow`            | `intensity`, `glow_radius`, `glow_quality`, `glow_gamma`, `glow_alpha` |
| `Vignette`        | `vignette_edges`, `vignette_sharpness` |
| `Contrast`        | `intensity`, `brightness` |
| `LinearBlur`      | `linear_blur_vector` |
| `Colourise`       | `intensity`, `color`, `tint` |

#### General Math
#### `ilerp(start, end, value)`

Returns how far `value` lies between `start` and `end`, as a normalized value from 0 to 1.

| Parameter | Description |
|-----------|-------------|
| `start` | The start of the range |
| `end` | The end of the range |
| `value` | The value to normalize between `start` and `end` |

Returns 0 if the calculation would result in `NaN` (e.g., when `a == b`).

- `lerp()` - Linear interpolation between two values
- `clamp()` - Constrain a value between minimum and maximum
- `min()` - Return the smallest value
- `max()` - Return the largest value

#### Collision Detection
- `point_in_rectangle()` - Check if a point is within a rectangular area

#### Array Functions
- `array_length()` - Get the length of an array
- `array_push()` - Add an element to the end of an array
- `array_create()` - Creates an array
- `array_delete()` - Deletes a certain amount of elements from an array at a certain position
- `array_resize()` - Resizes an array to a new size

#### Debug Functions
#### `debug_log(data)`
Logs a string to the ingame console.

| Parameter | Description |
|-----------|-------------|
| `data` | The thing to log |

#### `debug_log_warning(data)`
Logs a warning to the ingame console.

| Parameter | Description |
|-----------|-------------|
| `data` | The thing to log |

#### `debug_log_error(data)`
Logs an error to the ingame console.

| Parameter | Description |
|-----------|-------------|
| `data` | The thing to log |

---

## Constants

### Room Dimensions
- `room_width` - 1920
- `room_height` - 1080

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
- `level_color` - The current level's theme color (set with `set_level_color`).
- `delta_time` (delta_time) - Time elapsed since the last frame (delta_time / 1000)
- `player_sprites` - A struct containing player sprites with `inside`, `outside`, and `outline` values
- `player_count` - The amount of players (1-4)

### Number Constants
- `pi` - 3.141592653589793280

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
