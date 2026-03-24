---
title: "Scripts"
---
# Folder Structure

## Air Hockey
* **Goal Control** — Detects ball collision with goal colliders ("leftgoal", "rightgoal"), plays goal sound effect
* **Wall Control** — Plays boundary sound effect when the ball hits a wall
* **Hockey Ctr (hockeyctr.cs)** — Core ball controller: server-authoritative physics, client-side prediction, server reconciliation with exponential blending, snapshot buffering for interpolation, and metrics logging
* **Hockey Network Transform** — Extends NetworkTransform to provide position-update callbacks to hockeyctr for snapshot buffering
* **Game Manager** — Networked match lifecycle: start/stop, score tracking (NetworkVariable), countdown timer, difficulty mode sync, room owner tracking
* **Dedicated Server Lifecycle** — DS process management: heartbeat reporting to lobby (5s interval), idle shutdown (10s with no players), lobby status updates
* **Paddle Spawn Bridge** — Server-authoritative paddle spawning/despawning via RPCs
* **Instructions** — Loads tutorial scene corresponding to number keys [0-8] (for playtesting only)
* **Player Control** — Player input and control management
* **Lock Rotation 2D** — Constrains 2D rotation on game objects

## Networking
* **Simple Lobby Manager** — HTTP lobby integration using the network-api SDK: room creation/joining, connection approval with token validation, UI state management
* **Relay Manager** — Toggles between Unity Relay Service (online) and LAN direct mode (UnityTransport)
* **Network Latency UI** — RTT measurement via ping/pong RPCs, updated every 1 second, accessible via `NetworkLatencyUI.CurrentRTT`
* **Network Manager UI** — UI for starting/joining network sessions
* **Lobby List Single UI** — UI component for displaying a single room in the lobby list
* **Lobby Player Single UI** — UI component for displaying a player in the room
* **Networked Circle Controller** — Abstract base class for paddle position sources (mouse or ROS)
* **Pos Message Networked** — ROS-based paddle position input for production hardware (extends NetworkedCircleController)

## Paddle & Input
* **Circle Controller** — Paddle representation: collision detection with ball, hit velocity calculation, cooldown enforcement (1 RTT between hits), display name sync via NetworkVariable
* **Position Simulator** — Mouse-based paddle input for testing (extends NetworkedCircleController)
* **Position Source Applier** — Applies position from selected source to paddle transform
* **Position Source Toggle UI** — UI for switching between position input sources

## Trace & Replay
* **Trace Session** — Manager for recording/replaying game inputs as CSV
* **Trace Recorder** — Writes per-tick trace commands (paddle positions, hits, resets) to disk
* **Trace Input Provider** — Static helper to get/record/replay inputs
* **Trace Replayer** — Replays recorded trace data for deterministic playback

## Debug & Features
* **Network Features Controller** — Runtime toggles for enabling/disabling client-side prediction and snapshot interpolation

## Shared / Utility
* **Camera Adjust** — Calibrates camera position for projector display
* **Audio Manager** — Handles audio logic (boundary, goal, win, start sounds)
* **CV Dimensions Override** — Override computer vision field dimensions
* **gameMgr** — Non-networked game parameters (ball speed, radius, friction, goal sizes per difficulty mode)

## Shapes Catcher (Legacy, not yet networked)
* **Emitter** — Spawns random shapes at a specified time interval in random directions to both sides of the field
* **Shape Class** — Defines the three different shapes (Circle, Square, Triangle)
* **Shape Behavior** — Shape disappears after specified time passes or collides with player and handles player score
* **Switch Shape** — Uses keyboard input for switching shape (for playtesting only)
* **Timer** — Handles timer and game logic when timer stops
* **Shapes Game Manager** — Handles game rules including scores, winning / losing, timer
* **Pos Message SC** — Separate position message script for Shapes Catcher game
