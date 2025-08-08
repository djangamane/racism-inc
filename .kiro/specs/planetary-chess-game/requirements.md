# Requirements Document

## Introduction

Planetary Chess: Yurugu Edition is a turn-based strategy game built with Phaser 3 and TypeScript that simulates the resistance of indigenous African civilizations against colonial forces. The game features a map-based interface with regions, resource management, event-driven gameplay, AI opponents, and multiple victory/defeat conditions. Players must maintain four core metrics (spiritual integrity, economic independence, cultural immunity, and social cohesion) while managing resources and responding to colonizer threats through strategic decision-making.

## Requirements

### Requirement 1: Core Game Architecture

**User Story:** As a developer, I want a robust technical foundation using Phaser 3, TypeScript, and modern web technologies, so that the game is maintainable, performant, and deployable across multiple platforms.

#### Acceptance Criteria

1. WHEN the project is initialized THEN the system SHALL use Phaser 3 with TypeScript as the core engine
2. WHEN building the project THEN the system SHALL use Vite as the bundler and development server
3. WHEN managing game state THEN the system SHALL use XState for finite state machine management
4. WHEN storing game data THEN the system SHALL use JSON/YAML format for content-driven configuration
5. WHEN handling audio THEN the system SHALL integrate Howler.js for sound management
6. WHEN preparing for mobile deployment THEN the system SHALL support Capacitor for PWA wrapping
7. WHEN operating offline THEN the system SHALL implement PWA capabilities with service worker and IndexedDB

### Requirement 2: Map System and Regional Management

**User Story:** As a player, I want to interact with a visual map containing distinct regions with different properties, so that I can make strategic territorial decisions.

#### Acceptance Criteria

1. WHEN the map loads THEN the system SHALL display 8 distinct regions from Tiled JSON format
2. WHEN a player clicks a region THEN the system SHALL select that region and show available actions
3. WHEN displaying regions THEN each region SHALL have properties: id, name, biome, risk level, owner, and resources
4. WHEN rendering the map THEN the system SHALL use one tile layer for terrain and object layer for region interactions
5. WHEN a region changes ownership THEN the system SHALL update visual indicators accordingly
6. IF a region has special properties THEN the system SHALL display appropriate visual overlays or effects

### Requirement 3: Game State and Metrics Management

**User Story:** As a player, I want to track my civilization's core metrics and resources, so that I can make informed strategic decisions.

#### Acceptance Criteria

1. WHEN the game starts THEN the system SHALL initialize four core metrics: spiritualIntegrity, economicIndependence, culturalImmunity, and socialCohesion
2. WHEN displaying the HUD THEN the system SHALL show current values for all metrics and currencies
3. WHEN player actions occur THEN the system SHALL update metrics: faith, culturePoints, defense, food, and tradeGoods accordingly
4. WHEN metrics change THEN the system SHALL persist changes to the game state
5. IF any core metric falls below 10 THEN the system SHALL trigger a lose condition
6. WHEN the game state updates THEN the system SHALL auto-save progress to IndexedDB

### Requirement 4: Turn-Based Game Loop

**User Story:** As a player, I want a structured turn-based system that alternates between player actions and AI responses, so that gameplay feels strategic and paced.

#### Acceptance Criteria

1. WHEN the game begins THEN the system SHALL initialize the game loop state machine with states: playerTurn, aiTurn, eventsCheck, tickEnd
2. WHEN it's the player's turn THEN the system SHALL enable player actions and display "END TURN" button
3. WHEN the player ends their turn THEN the system SHALL transition to aiTurn state
4. WHEN in aiTurn state THEN the system SHALL execute colonizer AI actions automatically
5. WHEN AI turn completes THEN the system SHALL check for random events in eventsCheck state
6. WHEN all turn phases complete THEN the system SHALL increment turn counter and return to playerTurn
7. IF win/lose conditions are met THEN the system SHALL transition to appropriate end state

### Requirement 5: Event System and Player Choices

**User Story:** As a player, I want to encounter meaningful events with multiple choice options that affect my civilization, so that gameplay feels dynamic and consequential.

#### Acceptance Criteria

1. WHEN events are triggered THEN the system SHALL load event data from JSON files in data/events/ directory
2. WHEN an event occurs THEN the system SHALL display a modal with event title, description, and choice options
3. WHEN a player selects a choice THEN the system SHALL apply immediate effects to metrics and currencies
4. WHEN choices have risk factors THEN the system SHALL calculate and potentially apply negative consequences
5. IF events have long-term effects THEN the system SHALL track and apply them over multiple turns
6. WHEN events have conditions THEN the system SHALL only trigger events when conditions are met
7. WHEN an event resolves THEN the system SHALL close the modal and continue the game loop

### Requirement 6: Colonizer AI System ("Yurugu Engine")

**User Story:** As a player, I want to face an intelligent AI opponent that adapts its strategy based on my weaknesses, so that the game remains challenging and engaging.

#### Acceptance Criteria

1. WHEN it's the AI's turn THEN the system SHALL analyze player metrics to determine the lowest values
2. WHEN spiritualIntegrity is lowest THEN the AI SHALL prefer missionary-type actions
3. WHEN economicIndependence is lowest THEN the AI SHALL focus on trade domination tactics
4. WHEN culturalImmunity is lowest THEN the AI SHALL prioritize school/propaganda actions
5. WHEN socialCohesion is lowest THEN the AI SHALL employ divide-and-rule strategies
6. WHEN AI actions fail THEN the system SHALL increase the Utamaroho (aggression) scalar
7. WHEN aggression increases THEN the AI SHALL escalate through states: probe → pressure → exploit → assault → concede
8. WHEN AI controls Spirit Lake region THEN the system SHALL trigger a lose condition for the player

### Requirement 7: Upgrade System ("Wisdom Grid")

**User Story:** As a player, I want to purchase upgrades that improve my civilization's capabilities, so that I can customize my strategy and counter threats.

#### Acceptance Criteria

1. WHEN accessing upgrades THEN the system SHALL display a hex grid interface with Adinkra/Ifá motifs
2. WHEN displaying upgrades THEN each upgrade SHALL show category (spiritual, community, knowledge, defense), cost, and effects
3. WHEN a player has sufficient resources THEN the system SHALL allow purchase of available upgrades
4. WHEN an upgrade is purchased THEN the system SHALL apply its effects to the game state and mark it as owned
5. IF upgrades have prerequisites THEN the system SHALL only show available upgrades based on unlock conditions
6. WHEN hovering over upgrades THEN the system SHALL display tooltips with detailed information

### Requirement 8: Victory and Defeat Conditions

**User Story:** As a player, I want clear win/lose conditions that create meaningful goals and consequences, so that each game has a definitive outcome.

#### Acceptance Criteria

1. WHEN the player controls 5 or more regions by turn 15 THEN the system SHALL trigger a victory condition
2. WHEN any core metric falls below 10 THEN the system SHALL trigger a defeat condition
3. WHEN the colonizer controls Spirit Lake region THEN the system SHALL trigger a defeat condition
4. WHEN win/lose conditions are met THEN the system SHALL display an appropriate end screen with summary
5. WHEN the game ends THEN the system SHALL show final metrics, turn count, and key decisions made
6. WHEN on the end screen THEN the system SHALL provide options to restart or return to menu

### Requirement 9: Save System and Data Persistence

**User Story:** As a player, I want my game progress to be automatically saved and recoverable, so that I can continue playing across sessions.

#### Acceptance Criteria

1. WHEN each turn ends THEN the system SHALL automatically save the complete game state to IndexedDB
2. WHEN the player manually saves THEN the system SHALL create a named save slot with timestamp
3. WHEN loading a game THEN the system SHALL restore all game state including metrics, regions, turn count, and AI state
4. WHEN save data exists THEN the system SHALL offer to continue the previous game from the main menu
5. IF save data is corrupted THEN the system SHALL handle errors gracefully and offer to start a new game
6. WHEN multiple save slots exist THEN the system SHALL allow players to choose which save to load

### Requirement 10: Mobile and PWA Support

**User Story:** As a player, I want to play the game on mobile devices and offline, so that I can enjoy the experience anywhere.

#### Acceptance Criteria

1. WHEN the game is built THEN the system SHALL generate a Progressive Web App with offline capabilities
2. WHEN assets are loaded THEN the system SHALL cache images, audio, maps, and data files for offline use
3. WHEN running on mobile THEN the system SHALL provide touch-friendly controls and responsive UI
4. WHEN wrapped with Capacitor THEN the system SHALL support deployment to Android and iOS app stores
5. WHEN offline THEN the system SHALL function fully without internet connectivity
6. WHEN returning online THEN the system SHALL sync any analytics or optional data