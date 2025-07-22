# Shaper Documentation

> **Guide for creating objects in Shaper**

## Overview

**code.meow** contains the logic for your attack patterns and object behaviors. This file defines how your objects function.

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
