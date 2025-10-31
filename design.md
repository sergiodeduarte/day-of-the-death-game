# Design Document

## Overview

Calavera Runner is implemented as a single-file HTML5 browser game using vanilla JavaScript and Canvas API. The architecture follows a component-based entity system with a fixed-timestep game loop, emphasizing simplicity and performance. The visual design uses pure black-and-white retro ASCII aesthetics to honor Day of the Dead themes while maintaining technical simplicity.

## Architecture

### Core Architecture Pattern
- **Game Loop Pattern**: Fixed timestep with requestAnimationFrame for consistent 60 FPS performance
- **Entity Component System**: Simple object-based entities with update/render methods
- **State Machine**: Three distinct states (MENU, PLAYING, GAMEOVER) with clean transitions
- **Single File Architecture**: All HTML, CSS, and JavaScript contained in one index.html file

### Technology Stack
- **Language**: Vanilla JavaScript (ES6+)
- **Rendering**: HTML5 Canvas 2D Context
- **Input**: Keyboard Event API
- **Timing**: requestAnimationFrame with delta time calculations
- **No External Dependencies**: Pure browser APIs only

### Performance Design
- **Target Frame Rate**: 60 FPS constant
- **Canvas Resolution**: 800x480 pixels (4:3 aspect ratio)
- **Memory Management**: Object pooling for obstacles to minimize garbage collection
- **Culling**: Off-screen entities removed from update/render loops

## Components and Interfaces

### Game Class
```javascript
class Game {
  // Core game state management
  constructor(canvas)
  init()
  gameLoop(timestamp)
  update(deltaTime)
  render()
  
  // State transitions
  startGame()
  endGame()
  restartGame()
  
  // Entity management
  spawnObstacle()
  spawnCollectible()
  updateEntities(deltaTime)
  checkCollisions()
}
```

### Player Entity
```javascript
class Player {
  // Properties: x, y, width, height, velocityY, lane, isAlive
  // Methods: update(deltaTime), render(ctx), moveUp(), moveDown()
  
  // Lane-based movement system (3 lanes: top, middle, bottom)
  // Smooth transitions between lanes with easing
  // Fixed horizontal position, vertical movement only
}
```

### Obstacle Entity
```javascript
class Obstacle {
  // Properties: x, y, width, height, type, speed, active
  // Methods: update(deltaTime), render(ctx), isOffScreen()
  
  // Types: 'candle', 'marigold', 'papel'
  // Horizontal scrolling movement from right to left
  // Type-specific rendering and collision bounds
}
```

### Collectible Entity
```javascript
class Collectible {
  // Properties: x, y, width, height, value, active
  // Methods: update(deltaTime), render(ctx), collect()
  
  // Ofrenda items worth 10 points each
  // Same movement pattern as obstacles
  // Non-blocking collision (pass-through)
}
```

### Input Manager
```javascript
class InputManager {
  // Keyboard state tracking
  // Key mapping: Arrow keys, WASD, Space
  // Event listeners for keydown/keyup
  // State queries: isPressed(key), wasPressed(key)
}
```

### Renderer
```javascript
class Renderer {
  // Canvas context management
  // Drawing primitives for retro aesthetic
  // Text rendering for UI elements
  // Screen clearing and buffer management
}
```

## Data Models

### Game State Object
```javascript
gameState = {
  currentState: 'MENU' | 'PLAYING' | 'GAMEOVER',
  score: number,
  highScore: number,
  gameTime: number,
  difficultyMultiplier: number,
  obstacleSpeed: number,
  spawnTimer: number,
  lastSpawnTime: number
}
```

### Entity Base Structure
```javascript
entity = {
  x: number,           // Horizontal position
  y: number,           // Vertical position  
  width: number,       // Collision width
  height: number,      // Collision height
  velocityX: number,   // Horizontal velocity
  velocityY: number,   // Vertical velocity
  active: boolean,     // Entity lifecycle state
  type: string         // Entity classification
}
```

### Lane System
```javascript
lanes = {
  TOP: { y: 120, index: 0 },
  MIDDLE: { y: 240, index: 1 },
  BOTTOM: { y: 360, index: 2 }
}
```

## Visual Design System

### Color Palette
- **Background**: #000000 (pure black)
- **Foreground**: #FFFFFF (pure white)
- **Monochrome**: No additional colors for retro aesthetic

### Typography and Symbols
- **Player Character**: "☠" (skull emoji) or custom ASCII art
- **Obstacles**: 
  - Candle: "█" (solid block)
  - Marigold: "✿" (flower symbol)
  - Papel Picado: "▄▄▄" (block pattern)
- **Collectibles**: "●" (solid circle) for pan de muerto
- **UI Font**: Monospace font for consistent character spacing

### Layout Design
```
┌────────────────────────────────────┐
│ SCORE: 0000        HIGH: 0000      │ ← HUD (fixed position)
│                                    │
│  ☠                                 │ ← Player (lane system)
│         █  ✿     ▄▄▄               │ ← Obstacles (scrolling)
│                                    │
│━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━│ ← Ground reference
└────────────────────────────────────┘
```

### Animation System
- **Player**: Simple 2-frame idle animation (optional)
- **Obstacles**: Static rendering for performance
- **Transitions**: Smooth lane changes with easing functions
- **Effects**: Minimal particle effects on collision (white dots)

## Error Handling

### Input Validation
- **Key Event Filtering**: Only process valid game keys (arrows, WASD, space)
- **State Validation**: Ignore input during invalid game states
- **Rapid Input Protection**: Debounce rapid key presses to prevent spam

### Game State Recovery
- **Collision Edge Cases**: Handle multiple simultaneous collisions gracefully
- **Entity Overflow**: Limit maximum active entities to prevent memory issues
- **Timer Drift**: Reset game timers on state transitions to prevent accumulation

### Browser Compatibility
- **Canvas Support Detection**: Fallback message for unsupported browsers
- **RequestAnimationFrame Polyfill**: Fallback to setTimeout for older browsers
- **Keyboard Event Normalization**: Handle browser-specific key code differences

### Performance Safeguards
- **Frame Rate Monitoring**: Detect and handle performance drops
- **Memory Leak Prevention**: Proper entity cleanup and object pooling
- **Tab Visibility**: Pause game loop when tab is not active

## Testing Strategy

### Unit Testing Approach
- **Entity Behavior**: Test individual entity update and render methods
- **Collision Detection**: Verify AABB collision accuracy with edge cases
- **Score Calculation**: Validate point awarding and accumulation logic
- **State Transitions**: Test game state changes and data persistence

### Integration Testing
- **Game Loop Integration**: Test complete update-render cycle
- **Input-to-Action**: Verify keyboard input translates to correct entity behavior
- **Spawn System**: Test obstacle and collectible generation timing
- **Difficulty Progression**: Validate speed increases and spawn rate changes

### Performance Testing
- **Frame Rate Consistency**: Monitor FPS during extended gameplay
- **Memory Usage**: Track entity creation and cleanup over time
- **Browser Compatibility**: Test across target browser versions
- **Load Time**: Measure initial page load and game initialization

### User Experience Testing
- **Control Responsiveness**: Verify input lag and movement smoothness
- **Visual Clarity**: Test readability of ASCII characters and UI elements
- **Difficulty Balance**: Validate progression curve and player feedback
- **Accessibility**: Ensure keyboard-only navigation works completely

### Edge Case Testing
- **Rapid State Changes**: Test quick menu navigation and restart cycles
- **Boundary Conditions**: Test player movement at screen edges
- **Collision Timing**: Test simultaneous collisions and collection events
- **Long Session Stability**: Test extended gameplay for memory leaks or drift

## Implementation Phases

### Phase 1: Core Engine Foundation
- Game loop with requestAnimationFrame
- Canvas setup and basic rendering
- State management system (menu/play/gameover)
- Input handling infrastructure

### Phase 2: Entity System
- Base entity class with update/render methods
- Player entity with lane-based movement
- Collision detection system (AABB)
- Entity lifecycle management

### Phase 3: Gameplay Mechanics
- Obstacle spawning and movement
- Collectible system implementation
- Score tracking and display
- Game over conditions and restart

### Phase 4: Polish and Balance
- Difficulty progression implementation
- Visual refinements and animations
- Performance optimization
- Cross-browser testing and fixes