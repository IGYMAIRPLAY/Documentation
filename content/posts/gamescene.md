---
title: "Game Scene"
---
## Scene Objects

### Networking
* **NetworkManager**
    * Unity Netcode for GameObjects NetworkManager component
    * Configured with UnityTransport (UDP)
    * Handles connection approval, player spawning, and tick synchronization (30 Hz)
* **SimpleLobbyManager**
    * Manages lobby UI and HTTP communication with the FastAPI lobby server
    * Handles room creation, joining, starting, and connection approval token validation
* **DedicatedServerLifecycle**
    * Present on dedicated server instances only
    * Manages heartbeat to lobby, idle shutdown, and graceful termination

### Cameras
* Calibrates camera position for projector display
* Left Camera - displays the left side of the field
* Right Camera - displays the right side of the field

### Paddles (CircleController)
* Client-owned NetworkObjects representing player paddles
* Reads position from either mouse input (PositionSimulator) or ROS/CV (PosMessageNetworked)
* Handles collision detection with ball and hit velocity calculation
* Display name synced via NetworkVariable

### Ball (hockeyctr)
* Server-authoritative Rigidbody2D physics simulation
* Position synced via HockeyNetworkTransform at 30 ticks/sec
* Clients use snapshot interpolation for smooth rendering
* Hit-applying clients use client-side prediction with reconciliation

### Field
* Draws field lines using LineRenderer

### Canvas
* Displays UI including timer, scores, and game mode
* Score and timer values driven by NetworkVariables from server
* Lobby UI panels for room browsing, creation, and player list

### AudioManager
* Handles audio logic including:
    * Boundary hit
    * Goal scored
    * Win celebration
    * Game start

### GameManager
* Networked match lifecycle controller
* Syncs game state via NetworkVariables: isRunning, syncedTime, syncedMode, syncedScoreLeft, syncedScoreRight
* Server-only write permission on all game state
* Tracks room owner client ID for permission checks
