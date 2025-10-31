# Requirements Document

## Introduction

Calavera Runner is a browser-based endless runner game featuring Day of the Dead themes. The player controls a sugar skull (calavera) navigating through obstacles while collecting offerings (ofrendas) for points. The game emphasizes simplicity with retro black-and-white ASCII graphics and vanilla JavaScript implementation.

## Glossary

- **Calavera**: Sugar skull character controlled by the player
- **Ofrendas**: Collectible offerings that provide bonus points
- **Game_System**: The complete browser-based game application
- **Player_Entity**: The calavera character object with movement and collision properties
- **Obstacle_Entity**: Moving hazards that end the game on collision
- **Canvas_Renderer**: HTML5 Canvas-based rendering system
- **Game_Loop**: Main execution cycle handling input, updates, and rendering

## Requirements

### Requirement 1

**User Story:** As a player, I want to control a calavera character that moves vertically, so that I can navigate through obstacles in the game world.

#### Acceptance Criteria

1. WHEN the player presses the up arrow key or W key, THE Player_Entity SHALL move upward at 250 pixels per second
2. WHEN the player presses the down arrow key or S key, THE Player_Entity SHALL move downward at 250 pixels per second
3. THE Player_Entity SHALL remain within the vertical boundaries of the game canvas
4. THE Player_Entity SHALL maintain a fixed horizontal position on the left side of the screen
5. THE Player_Entity SHALL have a hitbox of 20x20 pixels for collision detection

### Requirement 2

**User Story:** As a player, I want obstacles to continuously spawn and move across the screen, so that I have challenges to avoid during gameplay.

#### Acceptance Criteria

1. THE Game_System SHALL spawn new Obstacle_Entity objects every 1.5 to 2 seconds
2. THE Obstacle_Entity SHALL move from right to left at 150 pixels per second
3. WHEN an Obstacle_Entity moves beyond the left edge of the screen, THE Game_System SHALL remove it from active entities
4. THE Game_System SHALL randomly select obstacle types from candles, marigolds, and papel picado banners
5. THE Obstacle_Entity SHALL have collision boundaries appropriate to their visual representation

### Requirement 3

**User Story:** As a player, I want the game to detect when I collide with obstacles, so that the game ends appropriately when I fail to avoid them.

#### Acceptance Criteria

1. THE Game_System SHALL continuously check for collisions between Player_Entity and all active Obstacle_Entity objects
2. WHEN a collision is detected between Player_Entity and any Obstacle_Entity, THE Game_System SHALL transition to game over state
3. THE Game_System SHALL use Axis-Aligned Bounding Box collision detection with 2-3 pixel tolerance
4. THE Game_System SHALL stop all entity movement when collision occurs
5. THE Game_System SHALL display the final score when game over state is reached

### Requirement 4

**User Story:** As a player, I want to collect ofrendas for bonus points, so that I can achieve higher scores during gameplay.

#### Acceptance Criteria

1. THE Game_System SHALL spawn collectible ofrendas every 3 to 4 seconds
2. WHEN Player_Entity overlaps with an ofrenda, THE Game_System SHALL award 10 points and remove the ofrenda
3. THE Game_System SHALL allow ofrendas to pass through without ending the game if not collected
4. THE Game_System SHALL move ofrendas at the same speed as obstacles
5. THE Game_System SHALL display collected ofrendas in the score calculation

### Requirement 5

**User Story:** As a player, I want to see my current score and track my progress, so that I can measure my performance and improvement.

#### Acceptance Criteria

1. THE Game_System SHALL award 1 point per second of survival time
2. THE Game_System SHALL display the current score in the top-left corner during gameplay
3. THE Game_System SHALL track and display the highest score achieved in the current session
4. THE Game_System SHALL calculate bonus points based on obstacles successfully avoided
5. THE Game_System SHALL show the final score on the game over screen

### Requirement 6

**User Story:** As a player, I want the game difficulty to increase over time, so that the challenge remains engaging as I improve.

#### Acceptance Criteria

1. THE Game_System SHALL increase obstacle speed by 10% every 30 seconds of gameplay
2. THE Game_System SHALL cap maximum obstacle speed at 300 pixels per second
3. THE Game_System SHALL gradually decrease spawn intervals to create more frequent obstacles
4. THE Game_System SHALL maintain the difficulty progression throughout a single game session
5. THE Game_System SHALL reset difficulty progression when starting a new game

### Requirement 7

**User Story:** As a player, I want clear game states with appropriate menus, so that I can start, play, and restart the game easily.

#### Acceptance Criteria

1. THE Game_System SHALL display a main menu with game title and start instructions
2. WHEN the player presses the space key on the main menu, THE Game_System SHALL transition to playing state
3. THE Game_System SHALL show a game over screen with score and restart option when the game ends
4. WHEN the player presses space on the game over screen, THE Game_System SHALL restart the game
5. THE Game_System SHALL maintain distinct visual layouts for menu, playing, and game over states

### Requirement 8

**User Story:** As a player, I want the game to run smoothly in my browser, so that I have a responsive and enjoyable gaming experience.

#### Acceptance Criteria

1. THE Game_System SHALL maintain 60 frames per second during gameplay
2. THE Canvas_Renderer SHALL use HTML5 Canvas API for all visual output
3. THE Game_Loop SHALL use requestAnimationFrame for consistent timing
4. THE Game_System SHALL function in Chrome 90+, Firefox 88+, Safari 14+, and Edge 90+
5. THE Game_System SHALL load completely within 2 seconds on a 3G connection