---
title: "unity-networking Documentation"
---
## Introduction

Welcome to the unity-networking documentation! This is the repository for the Unity part of the [iGYM system](https://www.igym.solutions/). iGYM aims to build games that are inclusive and fun for people of all abilities. We use projectors to display a virtual game field for players to physically interact with. Through the implementation of peripersonal boundaries and kick buttons, we level the playing field between people with motor disabilities and their non-disabled peers.

The game uses **Unity Netcode for GameObjects (NGO)** for multiplayer networking with a **dedicated server architecture**. A Python FastAPI lobby server (see [network-api](/posts/networking/)) handles matchmaking, room management, and dedicated server process orchestration.

Currently implemented:
* [Air Hockey](/posts/airhockey/) — Fully networked with dedicated server, client prediction, and server reconciliation
* [Shape Catcher](/posts/shapecatcher/) — Legacy single-machine mode (not yet networked)

## System Requirements
* Unity (with Netcode for GameObjects package)
* Windows 10/11
* Python 3.8+ (for lobby server)

## Development

### Branches
* **dedicated-server**: The primary development branch with full networking support (dedicated server, lobby integration, client prediction, interpolation)

### Architecture

The system separates networking into two planes:
* **Control Plane**: REST API lobby server (Python FastAPI) for matchmaking and DS orchestration
* **Game Plane**: Unity Netcode for GameObjects for real-time gameplay

See the [Networking Architecture](/posts/networking/) page for full details on:
- Lobby system endpoints and room lifecycle
- Dedicated server spawning and lifecycle management
- Connection approval and token authentication
- Client-side prediction and server reconciliation
- Snapshot interpolation
- State machine diagrams

### Game Structure
* [Tutorial scenes](/posts/tutorialscenes/)
* [Game scene](/posts/gamescene/)

### Folder Structure
* [Materials](/posts/materials/)
* [Prefabs](/posts/prefabs/)
* [Resources](/posts/resources/)
* [RosMessages](/posts/rosmessages/)
* [Scenes](/posts/scenes/)
* [Scripts](/posts/folderstructure/)

### Networking & Configuration
* [Networking Architecture](/posts/networking/) — Complete networking documentation with state machines
* [Tunable Parameters](/posts/parameters/) — All configurable parameters across every script, with default values and descriptions

### Latency & Diagnostics
* **RTT Measurement**: Ping/pong RPC-based, updated every 1 second via `NetworkLatencyUI.CurrentRTT`
* **Hit Position Discrepancy** `[HIT_POS_DISC]`: Logs visual vs. true position gap at hit time
* **Raw Position Delta** `[RAW_TRUE_POS_DELTA]`: Per-server-update position delta with tick gaps
* **Aggregate Stats** `[TRUE_POS_DELTA]`: Max deltas and buffer stats between RTT updates
* All metrics include ISO 8601 UTC timestamps, monotonic timer, RTT, and tick information

### Trace Recording & Replay
* **F7** = Start recording, **F8** = Replay, **F6** = Off
* Records per-tick: paddle positions, hit info (ID, velocity), ball resets
* Stored as CSV for deterministic replay and post-session analysis
* Useful for debugging network issues or analyzing player behavior

### Tutorial Implementation
* **Interactable**: These are the objects that the user interacts with throughout the tutorial, like buttons or tasks
* **Interactable Object**: These are the GameObjects that hold onto an Interactable to be found by the Tutorial Manager and enabled/disabled as necessary
* **Scene Object**: These are the Scriptable Objects that contain all of the information about a particular 'scene'. They hold lists of all of the Interactables that need to be enabled or disabled, what the background image needs to be, and what the next 'scene' will be after a particular Interactable Object has been selected.
* **Tutorial Manager**: This is the script that reads in all of the Interactable Objects and performs the operations outlined by the Scene Object. It does this via EventBus (Pub/Sub) events being published and methods being called via UnityEvents on the Interactable Objects.
* **Canvas Manager**: Currently just controls the setting of the image that the Tutorial Manager gets from the current Scene Object. This is done via EventBus with the Tutorial Manager publishing an UpdateImageEvent for the Canvas to respond to.

## Contact
helloigym@umich.edu
