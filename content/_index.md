---
title: "AiRPlay Wiki"
---

Welcome to the AiRPlay / iGYM documentation. iGYM builds inclusive games for people of all abilities using projectors to display a virtual game field that players physically interact with.

📧 Contact: helloigym@umich.edu

---

## Getting Started

New to the project? Start here:

- [**System Setup Guide (Windows)**](/posts/setup/) — Install all dependencies and get the system running
- [**ROS2 Emulation Documentation**](/posts/ros2-emulation/) — Running the ROS2 vision and input layer
- [**Unity Networking Project**](/posts/ros-test-out-doc/) — The main Unity game project

---

## System Architecture

The iGYM system has three main layers:

| Layer | Technology | Purpose |
|-------|-----------|---------|
| Computer Vision & Hardware | ROS2 | Camera-based player detection, Kalman filtering |
| Networking & Game Server | Unity Netcode for GameObjects | Dedicated server, client-side prediction, lobby system |
| Game Client | Unity | Rendering, input, local prediction |

Players connect through a lobby system that spawns a dedicated server. The server runs authoritative physics while clients predict locally and reconcile with server state.

---

## Documentation

### Unity Game (Networked)
- [Air Hockey](/posts/airhockey/) — Game rules, networking, hit system, and diagnostics
- [Networking Architecture](/posts/networking/) — Dedicated server model, prediction, reconciliation
- [Scenes Overview](/posts/scenes/) — Scene structure and flow
- [Game Scene](/posts/gamescene/) — Main game scene details
- [Tutorial Scenes](/posts/tutorialscenes/) — Non-networked tutorial content
- [Prefabs](/posts/prefabs/) — Prefab reference
- [Folder Structure](/posts/folderstructure/) — Project layout

### ROS2 System
- [ROS2 Emulation](/posts/ros2-emulation/) — Full ROS2 setup, nodes, and launch process
- [Player Detection Script](/posts/player-detection-script/) — Camera-based detection pipeline
- [ROS Messages](/posts/rosmessages/) — Custom message types
- [Shape Catcher](/posts/shapecatcher/) — Shape detection node
- [Parameters](/posts/parameters/) — Configurable system parameters
- [Button Script](/posts/button-script/) — BLE kick button node *(legacy, untested with current networking)*

### Other
- [Frontend](/posts/frontend/) — Web control interface *(not used with networked version)*
- [Materials](/posts/materials/) — Unity materials reference
- [Resources](/posts/resources/) — Additional resources

---

## Repositories

| Repo | Description | Links |
|------|-------------|-------|
| [unity-networking](https://github.com/IGYMAIRPLAY/unity-networking) | Main networked Unity game | [Docs](/posts/ros-test-out-doc/) |
| [ros2_emulation](https://github.com/IGYMAIRPLAY/ros2_emulation) | ROS2 vision & input layer | [Docs](/posts/ros2-emulation/) |
| [AIRPLAY-frontend](https://github.com/IGYMAIRPLAY/AIRPLAY-frontend) | Web control interface (legacy) | [Docs](/posts/frontend/) |
| [Airplay_KickButton](https://github.com/IGYMAIRPLAY/Airplay_KickButton) | BLE kick button (legacy) | README |
| [UWB](https://github.com/IGYMAIRPLAY/UWB) | Player identification via UWB | README |

---

## Onboarding

**New to Unity?**
- [Flappy Bird tutorial](https://www.youtube.com/watch?v=XtQMytORBmM) — good intro to Unity basics
- Familiarity with NetworkManager, NetworkVariables, and RPCs is recommended for the networked project

**New to ROS2?**
- [Brief ROS2 intro video](https://www.youtube.com/watch?v=7TVWlADXwRw)
- [Official ROS2 Foxy beginner tutorials](https://docs.ros.org/en/foxy/Tutorials.html) — complete CLI tools + Client libraries sections
- [System Setup Guide](/posts/setup/) — covers the full Windows install

**First hands-on step:** Switch `ros2_emulation` to the `video_test` branch and follow the [ROS2 Emulation docs](/posts/ros2-emulation/) to run the video version.
