# Calavera Runner - Day of the Dead Browser Game

A retro-style endless runner game featuring Day of the Dead themes, built with vanilla JavaScript and HTML5 Canvas.

## Project Overview

Calavera Runner is a browser-based endless runner where players control a sugar skull (calavera) navigating through obstacles while collecting ofrendas (offerings) for points. The game features:

- **Retro ASCII Graphics**: Pure black-and-white monochrome aesthetic
- **Simple Controls**: Arrow keys or WASD for vertical movement
- **Progressive Difficulty**: Speed increases and more obstacles over time
- **Cultural Themes**: Respectful Day of the Dead imagery and symbols
- **Zero Dependencies**: Pure vanilla JavaScript, single HTML file

## Specification Documents

This repository contains complete technical specifications following the Kiro spec-driven development methodology:

### 📋 [Requirements](specs/requirements.md)
Detailed user stories and acceptance criteria using EARS (Easy Approach to Requirements Syntax) format:
- 8 core requirements covering player movement, obstacles, collision detection, scoring, and performance
- Each requirement includes specific acceptance criteria with measurable conditions
- Complete glossary of technical terms and system components

### 🏗️ [Design](specs/design.md) 
Comprehensive technical architecture and implementation approach:
- Component-based entity system architecture
- Performance optimization strategies (60 FPS target, object pooling)
- Visual design system with ASCII character specifications
- Error handling and browser compatibility considerations
- Complete testing strategy for all game systems

### ✅ [Tasks](specs/tasks.md)
Actionable implementation plan with 10 main tasks and 23 sub-tasks:
- Incremental development approach from core engine to final polish
- Each task references specific requirements and includes implementation details
- Optional tasks marked for MVP-focused development
- Ready for execution by development teams or automated systems

## Game Features

### Core Gameplay
- **Player Character**: Sugar skull (☠) with lane-based vertical movement
- **Obstacles**: Candles (█), marigolds (✿), and papel picado banners (▄▄▄)
- **Collectibles**: Pan de muerto offerings (●) worth 10 points each
- **Scoring**: Time-based points (1/second) plus collection bonuses

### Technical Specifications
- **Canvas Size**: 800x480 pixels (4:3 aspect ratio)
- **Frame Rate**: 60 FPS constant performance target
- **Browser Support**: Chrome 90+, Firefox 88+, Safari 14+, Edge 90+
- **Load Time**: Under 2 seconds on 3G connections
- **Architecture**: Single HTML file with embedded CSS and JavaScript

### Difficulty Progression
- Obstacle speed increases 10% every 30 seconds
- Maximum speed cap at 300 pixels/second (2x starting speed)
- Spawn rate gradually increases for more frequent obstacles
- Progressive challenge while maintaining fair gameplay

## Development Approach

This project follows **spec-driven development** methodology:

1. **Requirements First**: Clear user stories with measurable acceptance criteria
2. **Design-Driven Architecture**: Technical decisions based on requirements analysis
3. **Task-Based Implementation**: Incremental coding tasks with clear objectives
4. **Quality Focus**: Built-in testing strategy and performance considerations

## Cultural Respect

The game respectfully incorporates Day of the Dead (Día de los Muertos) cultural elements:
- Celebratory tone focusing on remembrance and joy
- Authentic symbols used appropriately (calaveras, ofrendas, marigolds)
- Educational respect for Mexican cultural traditions
- Avoids scary or inappropriate representations

## Getting Started

To implement this game:

1. Review the [Requirements](specs/requirements.md) to understand all functional needs
2. Study the [Design](specs/design.md) for technical architecture decisions  
3. Follow the [Tasks](specs/tasks.md) for step-by-step implementation
4. Each task includes specific requirements references and implementation guidance

## File Structure

```
calavera-runner/
├── README.md                    # This overview document
├── specs/
│   ├── requirements.md          # Functional requirements and acceptance criteria
│   ├── design.md               # Technical architecture and design decisions
│   └── tasks.md                # Implementation plan and coding tasks
└── (implementation files to be created during development)
```

## Success Metrics

### Technical Success
- ✓ 60 FPS performance in all target browsers
- ✓ Sub-2-second load times
- ✓ Zero external dependencies
- ✓ Cross-browser compatibility

### Gameplay Success  
- ✓ Responsive controls with smooth movement
- ✓ Balanced difficulty progression
- ✓ Clear visual feedback and game states
- ✓ Engaging and replayable gameplay loop

## License

This specification is provided as-is for educational and development purposes. The game concept respects Day of the Dead cultural traditions and should be implemented with appropriate cultural sensitivity.

---

**Specification Version**: 1.0  
**Last Updated**: October 31, 2025  
**Status**: Ready for Implementation