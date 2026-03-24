---
title: "Air Hockey"
---
## Overview
Players interact with an augmented reality projected ball and attempt to score on the other side of the field. Players have expandable peripersonal boundaries that allow them to hit the ball by physically moving into it, the boundaries expand as they extend their limbs. Bluetooth kick buttons were an earlier input method for players with limited mobility but have not been used in a long time and are untested with the current networking architecture.

The game runs on a **dedicated server architecture** using Unity Netcode for GameObjects. The server is authoritative over ball physics, while clients use **client-side prediction** for responsive hit feedback and **interpolation** for smooth remote ball rendering.

## Networked Gameplay

### How It Works
1. Players join a match through the **lobby system** (HTTP-based room management)
2. The lobby spawns a **dedicated server** instance with authentication tokens
3. Each player connects as a client; the server validates their identity via connection approval
4. The server runs the physics simulation (Rigidbody2D) and broadcasts ball position at 30 ticks/sec
5. When a player hits the ball, the client **predicts** the result immediately and sends the hit to the server via RPC
6. The server applies the hit and broadcasts the authoritative state
7. If the client's prediction diverges from the server, it **reconciles** using exponential blending

### Client-Side Prediction
When a paddle collides with the ball:
- The client immediately applies the predicted velocity locally (no waiting for server)
- A `PredictHit()` call stores the predicted state and sets a prediction gate (~1 RTT)
- The hit is sent to the server via `ApplyVelocityServerRpc()`
- The server acknowledges the hit, and the client transitions back to following server authority

### Server Reconciliation
When the predicted position diverges from the server's truth:
- **Exponential blending** smoothly corrects the position
- **Max reconciliation speed** is clamped at 20 units/sec to prevent jarring snaps
- When the error drops below 0.05 units, the client snaps to the authoritative position

### Interpolation
For rendering the ball between server updates:
- A **buffered snapshot system** (circular buffer, max 128 entries) stores incoming server positions
- **Adaptive render delay** adjusts based on network jitter 
- Linear interpolation between snapshots prevents visible stuttering

## Game Modes

The game supports three difficulty modes, synced via `NetworkVariable`:

| Mode   | Ball Radius | Ball Speed | Friction |
|--------|-------------|-----------|----------|
| Easy   | 4.0         | 0.15      | 0.4      |
| Medium | 2.0         | 0.50      | 0.4      |
| Hard   | 1.0         | 0.75      | 0.4      |

Goal sizes also vary by mode, with easier modes having larger goals.

## Rules
After connecting to a match, the game begins when the server starts the timer. Players move onto the field and hit the ball by physically making contact with it, their peripersonal boundary expands as they extend their limbs. By hitting the ball into the goal on the other side, the player scores a point. Scores are synced via NetworkVariables and displayed to all clients in real time. At the end of the timer, the winner, or the side with the most points, will have a crown displayed on their field.

> **Note:** Bluetooth kick buttons were an earlier input method that has not been used in a long time. The kick button code path is likely untested with the current networking architecture and should be treated as unsupported until verified.

## Key Scripts

| Script | Purpose |
|--------|---------|
| **hockeyctr.cs** | Ball authority, client prediction, reconciliation, snapshot buffering, metrics logging |
| **CircleController.cs** | Paddle representation, collision detection, hit velocity calculation |
| **GameManager.cs** | Match lifecycle (start/stop), score tracking, timer, mode syncing via NetworkVariables |
| **HockeyNetworkTransform.cs** | Extends NetworkTransform to notify hockeyctr of authoritative position updates |
| **PaddleSpawnBridge.cs** | Server-authoritative paddle spawning/despawning via RPC |
| **GoalControl.cs** | Goal collision detection and score sound effects |
| **WallControl.cs** | Wall collision sound effects |

## Hit System
- **CircleController.OnCollisionEnter2D** detects paddle-ball contact
- Velocity is calculated as: `direction * (baseSpeed * kickFactor * ballSpeed)`
- A unique hit ID is assigned for latency correlation and metrics
- Cooldown enforcement: minimum 1 RTT between hits to prevent double-hits

## Metrics & Diagnostics
The game logs detailed networking metrics for performance analysis:
- **Hit Position Discrepancy** `[HIT_POS_DISC]`: Visual vs. true position gap at hit time, normalized by field diagonal
- **Raw Position Delta** `[RAW_TRUE_POS_DELTA]`: Per-update position delta with tick gaps and network stats
- **Aggregate Stats** `[TRUE_POS_DELTA]`: Max deltas, buffer counts, and network time offsets between RTT updates
- **RTT Measurement**: Ping/pong RPC-based, updated every 1 second, accessible via `NetworkLatencyUI.CurrentRTT`

## Trace Recording & Replay
The game includes a full trace recording system for debugging and analysis:
- **F7** = Start recording, **F8** = Replay, **F6** = Off
- Records per-tick: paddle positions, hit info (ID, velocity), ball resets
- Stored as CSV for deterministic replay