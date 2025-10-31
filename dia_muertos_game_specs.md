# Calavera Runner - Day of the Dead Browser Game
## Senior Software Architecture Specification

---

## 1. EXECUTIVE SUMMARY

**Game Title:** Calavera Runner  
**Genre:** Endless Runner / Dodge Game  
**Art Style:** Black & white retro ASCII/minimal graphics  
**Platform:** Browser-based (HTML5 Canvas)  
**Target:** Simple, functional, nostalgic gameplay

---

## 2. GAME CONCEPT

A side-scrolling endless runner where the player controls a sugar skull (calavera) navigating through the land of the dead, dodging obstacles like candles, marigold flowers, and papel picado banners while collecting ofrendas (offerings) for points.

---

## 3. TECHNICAL ARCHITECTURE

### 3.1 Technology Stack
- **Language:** Vanilla JavaScript (ES6+)
- **Rendering:** HTML5 Canvas API
- **No external dependencies** - pure browser APIs only
- **Storage:** In-memory state (no localStorage per requirements)

### 3.2 File Structure
```
/game
  index.html          # Single file application
  (all CSS and JS inline)
```

### 3.3 Architecture Pattern
- **Game Loop Pattern:** Fixed timestep with requestAnimationFrame
- **Entity Component Pattern:** Simple object-based entities
- **State Machine:** For game states (menu, playing, gameover)

---

## 4. CORE SYSTEMS

### 4.1 Game State Manager
```javascript
GameStates:
  - MENU: Initial screen with title and "Press SPACE to start"
  - PLAYING: Active gameplay
  - GAMEOVER: End screen with score and restart option
```

### 4.2 Game Loop
```javascript
Main Loop (60 FPS target):
  1. Handle input
  2. Update game state (delta time)
  3. Check collisions
  4. Render frame
  5. Request next frame
```

### 4.3 Entity System
```javascript
Base Entity Properties:
  - x, y (position)
  - width, height (hitbox)
  - velocity
  - active (boolean)
  - render() method
  - update(deltaTime) method
```

---

## 5. GAME ENTITIES

### 5.1 Player (Calavera)
**Properties:**
- Position: Fixed X (left side), variable Y
- Movement: Vertical only (up/down)
- Speed: 250 pixels/second
- Hitbox: 20x20 pixels
- Sprite: ASCII character "☠" or simple skull drawing

**Controls:**
- Arrow Up / W: Move up
- Arrow Down / S: Move down
- Space: Jump/Start game

### 5.2 Obstacles
**Types:**
1. **Candle** - Tall vertical obstacle
2. **Marigold** - Ground-level obstacle
3. **Papel Picado** - Hanging banner (top obstacle)

**Properties:**
- Spawn rate: Every 1.5-2 seconds
- Speed: 150 pixels/second (scrolling left)
- Random type selection
- Despawn when off-screen (x < -50)

### 5.3 Collectibles (Ofrendas)
**Types:**
- Pan de muerto (bread)
- Displayed as simple shapes or ASCII

**Properties:**
- Spawn rate: Every 3-4 seconds
- Value: 10 points each
- Speed: Same as obstacles
- Optional collection (pass through)

---

## 6. VISUAL DESIGN

### 6.1 Color Palette
```
Background: #000000 (black)
Foreground: #FFFFFF (white)
Accent: #FFFFFF (same, pure monochrome)
```

### 6.2 Visual Elements
**Rendering Style:**
- Simple geometric shapes (rectangles, circles)
- ASCII characters where appropriate
- 1-2px line weights
- No gradients or anti-aliasing for retro feel

**Screen Layout:**
```
┌────────────────────────────────────┐
│ SCORE: 0000        HIGH: 0000      │ ← HUD (top)
│                                    │
│  ☠                                 │ ← Player (left)
│         █  ✿     ▄▄▄               │ ← Obstacles (scrolling)
│                                    │
│━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━│ ← Ground line
└────────────────────────────────────┘
```

### 6.3 Animation
- Player: Simple 2-frame idle animation (optional)
- Obstacles: Static (no animation needed for v1)
- Background: Optional slow-scrolling pattern

---

## 7. GAME MECHANICS

### 7.1 Core Gameplay Loop
```
1. Player moves vertically in lanes (3 lanes: top, middle, bottom)
2. Obstacles scroll from right to left
3. Player dodges obstacles by switching lanes
4. Collect ofrendas for bonus points
5. Collision = Game Over
6. Score increases over time (1 point per second survived)
```

### 7.2 Difficulty Progression
- Every 30 seconds: Increase obstacle speed by 10%
- Maximum speed: 300 pixels/second (2x starting speed)
- Spawn rate gradually decreases (more obstacles)

### 7.3 Collision Detection
```javascript
Algorithm: Axis-Aligned Bounding Box (AABB)
  - Simple rectangle overlap detection
  - Check player hitbox against all active obstacles
  - 2-3px tolerance for fairness
```

### 7.4 Scoring System
```
Points awarded for:
  - Time survived: 1 point/second
  - Collecting ofrendas: 10 points each
  - Bonus at game over: (obstacles dodged × 2)
```

---

## 8. USER INTERFACE

### 8.1 Main Menu
```
╔════════════════════════════════════╗
║                                    ║
║        CALAVERA RUNNER             ║
║                                    ║
║            ☠  ☠  ☠                ║
║                                    ║
║     [PRESS SPACE TO START]         ║
║                                    ║
║    Use Arrow Keys or W/S to move   ║
║                                    ║
╚════════════════════════════════════╝
```

### 8.2 Game Over Screen
```
╔════════════════════════════════════╗
║                                    ║
║          GAME OVER                 ║
║                                    ║
║       Your Score: 1234             ║
║       High Score: 5678             ║
║                                    ║
║     [SPACE] Play Again             ║
║                                    ║
╚════════════════════════════════════╝
```

### 8.3 HUD (During Gameplay)
- Top-left: Current score
- Top-right: High score (session-based)
- Minimal, non-intrusive

---

## 9. AUDIO DESIGN (OPTIONAL - v1.1)

For initial version: **NO AUDIO**  
Future consideration: Simple beep/blip sound effects using Web Audio API

---

## 10. DATA STRUCTURES

### 10.1 Game State Object
```javascript
gameState = {
  currentState: 'MENU',
  score: 0,
  highScore: 0,
  gameTime: 0,
  difficultyMultiplier: 1.0,
  player: PlayerObject,
  obstacles: [ObstacleArray],
  collectibles: [CollectibleArray]
}
```

### 10.2 Player Object
```javascript
player = {
  x: 50,
  y: 240,  // Canvas center
  width: 20,
  height: 20,
  velocityY: 0,
  lane: 1,  // 0=top, 1=middle, 2=bottom
  isAlive: true
}
```

### 10.3 Obstacle Object
```javascript
obstacle = {
  x: 800,  // Spawn off-screen right
  y: [determined by type],
  width: 30,
  height: 40,
  type: 'candle|marigold|papel',
  speed: 150,
  active: true
}
```

---

## 11. PERFORMANCE REQUIREMENTS

### 11.1 Target Specifications
- **Frame Rate:** 60 FPS constant
- **Canvas Size:** 800x480 pixels (4:3 ratio)
- **Memory:** < 50MB during gameplay
- **Load Time:** < 2 seconds on 3G connection

### 11.2 Optimization Strategies
- Object pooling for obstacles (reuse instead of recreate)
- Cull off-screen entities from render loop
- Single canvas layer (no compositing needed)
- Minimal garbage collection (reuse objects)

---

## 12. BROWSER COMPATIBILITY

### 12.1 Target Browsers
- Chrome 90+ ✓
- Firefox 88+ ✓
- Safari 14+ ✓
- Edge 90+ ✓

### 12.2 Required APIs
- Canvas 2D Context
- requestAnimationFrame
- Keyboard Events
- Basic ES6 (const, let, arrow functions, classes)

---

## 13. DEVELOPMENT PHASES

### Phase 1: Core Engine (Day 1)
- [x] Game loop with fixed timestep
- [x] State management (menu/play/gameover)
- [x] Basic input handling
- [x] Canvas rendering setup

### Phase 2: Player & Movement (Day 1)
- [x] Player entity with lane-based movement
- [x] Smooth vertical transitions
- [x] Basic collision boundaries (screen edges)

### Phase 3: Obstacles & Collision (Day 2)
- [x] Obstacle spawning system
- [x] Scrolling behavior
- [x] AABB collision detection
- [x] Game over condition

### Phase 4: Scoring & Polish (Day 2)
- [x] Score tracking and display
- [x] High score persistence (session)
- [x] UI screens (menu, game over)
- [x] Difficulty progression

### Phase 5: Visual Polish (Day 3)
- [x] Retro aesthetic refinement
- [x] Simple animations
- [x] Visual feedback for collisions/collections
- [x] Final balancing

---

## 14. TESTING CHECKLIST

### 14.1 Functional Tests
- [ ] Player can move up/down smoothly
- [ ] Obstacles spawn at correct intervals
- [ ] Collisions detected accurately
- [ ] Score increments correctly
- [ ] Game restarts properly
- [ ] High score persists during session

### 14.2 Edge Cases
- [ ] Rapid input spam doesn't break game
- [ ] Tab switching pauses game correctly
- [ ] Window resize doesn't break layout
- [ ] Multiple collisions in single frame

### 14.3 Cross-Browser
- [ ] Works in Chrome
- [ ] Works in Firefox
- [ ] Works in Safari
- [ ] Mobile touch events (if applicable)

---

## 15. FUTURE ENHANCEMENTS (Post-v1)

**v1.1 Additions:**
- Sound effects (beep on collect/dodge)
- Particle effects on collision (white dots)
- Powerups (temporary invincibility)

**v1.2 Additions:**
- Multiple difficulty modes
- Different themed backgrounds
- Character selection (different calaveras)

**v2.0 Additions:**
- Local leaderboard
- Procedural background generation
- Boss encounters

---

## 16. CODE STRUCTURE EXAMPLE

```javascript
// High-level pseudocode structure
class Game {
  constructor(canvas) {
    this.canvas = canvas;
    this.ctx = canvas.getContext('2d');
    this.state = 'MENU';
    this.init();
  }

  init() {
    this.setupInput();
    this.player = new Player(50, 240);
    this.obstacles = [];
    this.spawnTimer = 0;
    this.lastTime = 0;
  }

  gameLoop(timestamp) {
    const deltaTime = (timestamp - this.lastTime) / 1000;
    this.lastTime = timestamp;

    this.update(deltaTime);
    this.render();
    
    requestAnimationFrame((t) => this.gameLoop(t));
  }

  update(dt) {
    if (this.state === 'PLAYING') {
      this.player.update(dt);
      this.updateObstacles(dt);
      this.checkCollisions();
      this.updateScore(dt);
    }
  }

  render() {
    this.clearScreen();
    if (this.state === 'MENU') this.renderMenu();
    if (this.state === 'PLAYING') this.renderGame();
    if (this.state === 'GAMEOVER') this.renderGameOver();
  }
}

class Player {
  constructor(x, y) {
    this.x = x;
    this.y = y;
    this.targetLane = 1;
    // ... other properties
  }

  update(dt) {
    // Smooth lane transitions
    // Update position
  }

  render(ctx) {
    // Draw skull character
  }
}

class Obstacle {
  constructor(type, x, y) {
    this.type = type;
    this.x = x;
    this.y = y;
    // ... other properties
  }

  update(dt, speed) {
    this.x -= speed * dt;
  }

  render(ctx) {
    // Draw obstacle based on type
  }
}
```

---

## 17. SUCCESS METRICS

### 17.1 Technical Success
- ✓ Game runs at 60 FPS
- ✓ No crashes or freezes
- ✓ Loads in under 2 seconds
- ✓ Works in all target browsers

### 17.2 Gameplay Success
- ✓ Controls feel responsive
- ✓ Difficulty curve is balanced
- ✓ Clear win/loss conditions
- ✓ Replayable and fun

### 17.3 Visual Success
- ✓ Achieves retro aesthetic
- ✓ Clear visual hierarchy
- ✓ Readable at all screen sizes
- ✓ Consistent Day of the Dead theme

---

## 18. DEPLOYMENT

**Hosting:** Any static file host (GitHub Pages, Netlify, Vercel)  
**Build Process:** None required (single HTML file)  
**CDN:** Not required  
**Dependencies:** None

---

## 19. MAINTENANCE PLAN

**v1.0:** Focus on stability and bug fixes  
**Monthly:** Review player feedback  
**Quarterly:** Consider feature additions  

---

## APPENDIX A: Day of the Dead Theme Elements

**Visual Motifs to Include:**
- Sugar skulls (calaveras) ☠
- Marigold flowers (cempasúchil) ✿
- Candles (velas) 🕯
- Papel picado (perforated paper) ▄▄▄
- Pan de muerto (bread) ●

**Cultural Respect:**
- Keep tone celebratory, not scary
- Focus on remembrance and joy
- Use authentic symbols appropriately

---

## APPENDIX B: ASCII Art Examples

```
Skull Options:
☠  ☺̴̢̧̱̰̲̦̤̫͖̰͖̟̬͙͔̅̈́̊̽͊̍͊̃̚͝͠  (╯°□°)╯

Obstacles:
█ (candle)
✿ (flower)
▄▄▄ (papel picado)
● (bread)
```

---

## FINAL NOTES

This specification prioritizes:
1. **Simplicity** - One file, no dependencies
2. **Functionality** - Working game first, polish second
3. **Retro aesthetic** - Black & white, minimal graphics
4. **Cultural authenticity** - Respectful Day of the Dead representation
5. **Buildability** - Clear enough for automated build tools

The game should be completable in 2-3 days by a competent developer or automated build system. All features are scoped to be achievable without external libraries or complex systems.

---

**Document Version:** 1.0  
**Last Updated:** October 31, 2025  
**Status:** Ready for Implementation