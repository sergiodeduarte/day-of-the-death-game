# Implementation Plan

- [ ] 1. Set up project structure and core game foundation
  - Create single HTML file with embedded CSS and JavaScript
  - Set up HTML5 Canvas element with proper dimensions (800x480)
  - Initialize basic CSS for full-screen canvas and retro styling
  - _Requirements: 8.2, 8.4_

- [ ] 2. Implement core game loop and state management
  - [ ] 2.1 Create Game class with requestAnimationFrame loop
    - Implement fixed timestep game loop with delta time calculation
    - Set up 60 FPS target with performance monitoring
    - _Requirements: 8.1, 8.3_
  
  - [ ] 2.2 Implement game state machine
    - Create state management for MENU, PLAYING, and GAMEOVER states
    - Add state transition methods (startGame, endGame, restartGame)
    - _Requirements: 7.1, 7.3, 7.5_
  
  - [ ] 2.3 Set up input handling system
    - Create keyboard event listeners for arrow keys, WASD, and space
    - Implement input state tracking and key press detection
    - _Requirements: 1.1, 1.2, 7.2, 7.4_

- [ ] 3. Create entity system and base classes
  - [ ] 3.1 Implement base Entity class
    - Create entity base class with position, dimensions, and velocity properties
    - Add update and render method interfaces
    - Implement basic collision bounds calculation
    - _Requirements: 1.4, 2.5, 3.3_
  
  - [ ] 3.2 Create Player entity with lane-based movement
    - Implement Player class extending base Entity
    - Add vertical movement with 250 pixels per second speed
    - Create lane system with smooth transitions between top, middle, bottom positions
    - Implement screen boundary constraints
    - _Requirements: 1.1, 1.2, 1.3, 1.4, 1.5_
  
  - [ ]* 3.3 Write unit tests for entity classes
    - Test Player movement boundaries and speed calculations
    - Test entity collision bounds and position updates
    - _Requirements: 1.1, 1.2, 1.3_

- [ ] 4. Implement obstacle system
  - [ ] 4.1 Create Obstacle entity class
    - Implement Obstacle class with horizontal movement (150 pixels/second)
    - Add obstacle type system (candle, marigold, papel picado)
    - Create off-screen detection and cleanup
    - _Requirements: 2.1, 2.2, 2.3, 2.4, 2.5_
  
  - [ ] 4.2 Build obstacle spawning system
    - Implement timed spawning every 1.5-2 seconds with randomization
    - Add random obstacle type selection
    - Create obstacle pool management for performance
    - _Requirements: 2.1, 2.3, 2.4_
  
  - [ ]* 4.3 Create obstacle system tests
    - Test spawning intervals and randomization
    - Test movement speed and cleanup behavior
    - _Requirements: 2.1, 2.2, 2.3_

- [ ] 5. Add collision detection and game over mechanics
  - [ ] 5.1 Implement AABB collision detection
    - Create collision detection function with 2-3 pixel tolerance
    - Add collision checking between player and all active obstacles
    - Implement collision response and game over trigger
    - _Requirements: 3.1, 3.2, 3.3, 3.4, 3.5_
  
  - [ ] 5.2 Create game over state handling
    - Implement game over state transition on collision
    - Stop all entity movement and display final score
    - Add restart functionality from game over screen
    - _Requirements: 3.4, 3.5, 7.3, 7.4_
  
  - [ ]* 5.3 Test collision detection accuracy
    - Test edge cases and boundary conditions for collisions
    - Verify tolerance settings work correctly
    - _Requirements: 3.1, 3.3_

- [ ] 6. Implement collectible system and scoring
  - [ ] 6.1 Create Collectible entity for ofrendas
    - Implement Collectible class with pass-through collision
    - Add spawning system every 3-4 seconds
    - Create collection detection and point awarding (10 points)
    - _Requirements: 4.1, 4.2, 4.3, 4.4, 4.5_
  
  - [ ] 6.2 Build comprehensive scoring system
    - Implement time-based scoring (1 point per second)
    - Add score display in top-left corner during gameplay
    - Create high score tracking for current session
    - Calculate and display bonus points for obstacles avoided
    - _Requirements: 5.1, 5.2, 5.3, 5.4, 5.5_
  
  - [ ]* 6.3 Create scoring system tests
    - Test point calculation accuracy for time and collections
    - Test high score persistence during session
    - _Requirements: 5.1, 5.2, 5.3_

- [ ] 7. Add difficulty progression system
  - [ ] 7.1 Implement dynamic difficulty scaling
    - Add obstacle speed increase (10% every 30 seconds)
    - Implement maximum speed cap at 300 pixels per second
    - Create spawn interval reduction for increased frequency
    - _Requirements: 6.1, 6.2, 6.3, 6.4, 6.5_
  
  - [ ]* 7.2 Test difficulty progression balance
    - Verify speed increases occur at correct intervals
    - Test maximum speed enforcement and spawn rate changes
    - _Requirements: 6.1, 6.2, 6.3_

- [ ] 8. Create visual rendering system
  - [ ] 8.1 Implement retro ASCII renderer
    - Create rendering functions for player skull character (☠)
    - Add obstacle rendering (█ for candle, ✿ for marigold, ▄▄▄ for papel picado)
    - Implement collectible rendering (● for pan de muerto)
    - Set up monochrome color scheme (black background, white foreground)
    - _Requirements: 8.2_
  
  - [ ] 8.2 Build UI rendering system
    - Create main menu screen with title and instructions
    - Implement game over screen with score display and restart option
    - Add HUD with current score and high score display
    - _Requirements: 7.1, 7.3, 7.5, 5.2, 5.3_
  
  - [ ]* 8.3 Add visual polish and animations
    - Implement smooth lane transition animations for player
    - Add simple collision feedback effects
    - Create visual consistency across all game states
    - _Requirements: 1.1, 1.2_

- [ ] 9. Optimize performance and browser compatibility
  - [ ] 9.1 Implement performance optimizations
    - Add object pooling for obstacles and collectibles
    - Implement entity culling for off-screen objects
    - Optimize render loop to maintain 60 FPS
    - _Requirements: 8.1, 8.5_
  
  - [ ] 9.2 Ensure cross-browser compatibility
    - Test and fix compatibility for Chrome 90+, Firefox 88+, Safari 14+, Edge 90+
    - Add requestAnimationFrame polyfill if needed
    - Normalize keyboard event handling across browsers
    - _Requirements: 8.4, 8.5_
  
  - [ ]* 9.3 Performance testing and monitoring
    - Add frame rate monitoring and performance metrics
    - Test memory usage during extended gameplay sessions
    - _Requirements: 8.1, 8.5_

- [ ] 10. Final integration and polish
  - [ ] 10.1 Integrate all systems and test complete gameplay loop
    - Wire together all game systems (player, obstacles, collectibles, scoring)
    - Test complete game flow from menu to game over and restart
    - Verify all requirements are met through end-to-end testing
    - _Requirements: All requirements 1.1-8.5_
  
  - [ ] 10.2 Final balancing and bug fixes
    - Adjust difficulty curve based on gameplay testing
    - Fix any remaining bugs or edge cases
    - Optimize game balance for enjoyable player experience
    - _Requirements: 6.1, 6.2, 6.3, 6.4_
  
  - [ ]* 10.3 Create comprehensive test suite
    - Build integration tests for complete game scenarios
    - Test edge cases like rapid input, tab switching, window resize
    - _Requirements: 8.1, 8.4, 8.5_