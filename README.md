# Party Killer
A 2-player local top-down shooter built in Unity 6. Compete in arena-style levels as "Party" vs "Police" in a disco-themed game featuring bouncing bullets and explosive action.

## Quick Start

### Prerequisites
- Unity 6000.3.12f1 (or later 6.x versions)
- Visual Studio or Rider (C# IDE)
- Windows/Mac/Linux (tested on Windows 11)

### Setup
1. Clone or extract the project
2. Open it in Unity Hub → Unity 6000.3.12f1
3. Wait for asset import and compilation
4. Open `Assets/Scenes/MAIN.unity`
5. Press Play

### Controls
**Player A (Party):**
- Left Stick = Move
- Right Stick = Aim
- Right Trigger = Shoot

**Player B (Police):**
- Left Stick = Move
- Right Stick = Aim
- Right Trigger = Shoot

Controller support (Xbox/PlayStation layout). Keyboard support via the New Input System.

## Gameplay Overview

### Concept
Two players spawn into randomized arena levels. One is "Party", the other is "Police". The first player to eliminate the opponent earns 1 point. Rounds continue infinitely with persistent score tracking.

### Core Mechanics
- **Movement:** Each player controls movement and aiming independently with a dual-stick layout
- **Shooting:** Limited ammo (max 3 bullets). Bullets auto-reload at 1 bullet/second
- **Bullet Physics:** Bullets bounce off walls twice, then explode on the 3rd impact. Instant kill on player contact
- **Bullet Destruction:** Two bullets colliding destroy each other
- **Arena Variety:** 11 arena levels (Level00-Level10) are loaded randomly; immediate repeats are avoided
- **Camera Shake:** Feedback on shooting, bouncing, and kills

### Win Condition
Eliminate your opponent to score points. There is no score cap—the game runs indefinitely.

## Project Structure

```
Assets/
├── Animation/        # Animator controllers & animation clips
├── Audio/            # Sound effects (.wav)
├── Scenes/           # Persistent MAIN scene + 11 arena levels
├── Scripts/          # C# gameplay logic (14 core files)
├── Prefabs/          # Player, bullet, death VFX, wall prefabs
├── Models/           # 3D wall models
├── Materials/        # URP materials
├── Sprites/          # 2D sprites & UI
├── Fonts/            # Oxanium font (TextMeshPro)
├── Controls.inputactions  # Input action mappings
└── Settings/         # URP rendering settings
```

## Core Systems

### GameManager (Singleton)
- Game state, player spawning, score tracking, level loading
- Handles player join events, death detection, and score updates
- Manages scene load/unload with fade animation

### PlayerController
- Movement via left stick, aiming via right stick
- Rigidbody-based physics movement
- Pauses input while waiting for players

### PlayerWeapon
- Ammo magazine (max 3), auto-reload at 1/second
- Real-time UI indicator (3 displayed bullets)
- Triggers camera shake when firing

### Bullet Physics
- Bounce counter (2 bounces before explosion)
- Collision detection: player (kill), bullet (destroy both), wall (bounce/explode)
- Instant explosion particle prefab on death

### AudioManager (Singleton, DontDestroyOnLoad)
- Plays named sounds ("Shoot", "Bounce", "Explode", "BigExplode", "Ready", "PowerDown", "PartyMusic")
- Volume/pitch variation for dynamic audio
- Automatically starts "PartyMusic" on startup

## Tech & Dependencies

| Dependency | Version | Purpose |
|-----------|---------|---------|
| Unity | 6000.3.12f1 | Engine |
| URP | 17.3.0 | Rendering |
| Input System | 1.19.0 | Controller/keyboard input |
| TextMeshPro | Included | UI text |
| Physics | Included | Rigidbody movement |
| AI Navigation | 2.0.11 | Navigation mesh (optional) |

**EZCameraShake** (third-party, included): Screen shake effects during gameplay events.

## Development Notes

- All physics uses Rigidbody with 3D colliders (Capsule for players, Sphere for bullets, Box for walls)
- Input is managed by Unity's New Input System with local PlayerInputManager
- Scenes use an additive loading pattern: persistent MAIN + additive level scenes
- No networking—local multiplayer only
- No UI menu/start screen—join-then-play flow is immediate

## Build

1. File → Build Settings
2. Add MAIN + Level00-Level10 scenes to Build (scene indices 0-11)
3. Configure URP pipeline assets in Quality settings
4. Select target platform (PC/Mac/Linux)
5. Build & Run

## Troubleshooting

**Game does not compile:**
- Ensure Input System 1.19.0 is installed via Package Manager
- Check URP asset assignments in Quality settings

**Players cannot join:**
- Verify `Controls.inputactions` is in the `Assets` root
- Confirm `PlayerInputManager` exists in the MAIN scene

**Missing audio:**
- Check `Assets/Audio/` contains `.wav` files
- Verify AudioManager has a populated `Sound[]` array in the inspector

**Levels do not load:**
- Confirm Level00-Level10 scenes exist and are listed in Build Settings in order
- Check scene index assignments match `GetRandomLevelIndex()` range (1-10)

## Credits

Developed using Brackeys tutorial patterns. Uses the EZCameraShake library for camera effects.

---

**Version:** 1.0  
**Engine:** Unity 6000.3.12f1  
**Genre:** Local Multiplayer, Top-Down Shooter
