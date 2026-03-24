---
title: "ros2-emulation Documentation"
---
## Introduction

Welcome to the ros2-emulation wiki! ros2-emulation is the repository for the ROS2 part of the entire [iGYM system](https://www.igym.solutions/). iGYM aims to build games that are inclusive and fun for people of all abilities. We use projectors to display a virtual game field for players to physically interact with. Through the implementation of peripersonal boundaries and kick buttons, we level the playing field between people with motor disabilities and their non-disabled peers.

This repository implements:
- **Player detection** using OpenCV with Kalman filtering and Hungarian algorithm tracking
- **BLE button input** via SimpleBLE for wireless kick controllers
- **ROS2-Unity TCP bridge** for real-time communication with the game engine

The current most up-to-date branch for the onsite play field is the **OnsiteVersionCuda_Multithreads** branch.

## Requirements
* Windows 10/11
* ROS2 Foxy
* OpenCV 4.11.0
* FLIR Spinnaker SDK (for FLIR camera) or USB camera
* SimpleBLE (for BLE button support)
* Eigen 3.4.0 (for Kalman filter linear algebra)

## Installation

### ROS2 Foxy

[Here](https://docs.ros.org/en/foxy/Installation/Windows-Install-Binary.html) is the installation for ROS2 Foxy official document (You might encounter some errors later while building the workspace)

[Here](https://ms-iot.github.io/ROSOnWindows/GettingStarted/SetupRos2.html) is an alternative way to install (only in disk C) (recommended)

### Button

BLE button support requires SimpleBLE library. The button node scans for devices by MAC address and reads kick state from BLE characteristic `beb5483e-36e1-4688-b7f5-ea07361b26a8`.

## Overview

### System Architecture

The ROS2 side consists of four packages:

| Package | Type | Purpose |
|---------|------|---------|
| **ap_interfaces** | C++ (ament_cmake) | Core CV player detection, BLE button input, custom message definitions |
| **ros_tcp_endpoint** | Python (ament_python) | TCP bridge between ROS2 and Unity (v0.7.0) |
| **unity_robotics_demo** | Python (ament_python) | Demo nodes for integration testing |
| **unity_robotics_demo_msgs** | C++ (ament_cmake) | Custom message/service definitions for demo |

### Data Flow

{{< mermaid >}}
flowchart TD
    CAM["FLIR / USB Camera"]
    BLE["BLE Controllers *(legacy)*"]
    PD["player_detection node"]
    BTN["button node *(legacy)*"]
    POS["pos_raw topic"]
    KICK["kick_size topic *(legacy)*"]
    TCP["ros_tcp_endpoint\nTCP:10000"]
    UNITY["Unity Game"]

    CAM --> PD --> POS --> TCP
    BLE --> BTN --> KICK --> TCP
    TCP --> UNITY
{{< /mermaid >}}

### Custom Messages

**ap_interfaces/Pos** (topic: `pos_raw`):
| Field | Type | Purpose |
|-------|------|---------|
| `total` | int8 | Number of detected players |
| `timestamp` | uint64 | Capture timestamp in ms |
| `x[32]` | float64[] | X positions (up to 32 players) |
| `y[32]` | float64[] | Y positions |
| `player_id[32]` | int8[] | Unique player IDs |
| `tag_id[32]` | string[] | RFID tag IDs |
| `size[32]` | float64[] | Player blob radius/size |

**ap_interfaces/Button** (topic: `kick_size`):
| Field | Type | Purpose |
|-------|------|---------|
| `kick[32]` | bool[] | Button state for up to 32 players |

**ap_interfaces/Score**:
| Field | Type | Purpose |
|-------|------|---------|
| `player_score[32]` | int16[] | Individual player scores |
| `game_score[32]` | int16[] | Game-level scores |

**ap_interfaces/AddThreeInts** (service):
| Field | Type | Purpose |
|-------|------|---------|
| ballspeed | float32 | Ball speed parameter |
| ballradius | float32 | Ball radius |
| goalsize | float32 | Goal size |
| friction | float32 | Friction coefficient |
| buttonspeed | float32 | Button expansion speed |
| capture | float32 | Capture flag |
| startstop | float32 | Start/stop flag |

### Running Procedure

Open a terminal:

`c:\opt\ros\foxy\x64\setup.bat`  Source the setup file

`cd C:\[Path To The Repo]\ros2_emulation\src`

`colcon build --merge-install --packages-select ap_interfaces` only build ap_interfaces or if you want to build all: `colcon build --merge-install` Everytime you make a change you need to rebuild.

`call install\setup.bat` Source the workspace

`ros2 launch ap_interfaces airplay_launch.py` to run both the player_detection and button nodes, or `ros2 run ap_interfaces player_detection` to run only the player_detection node

Open another terminal (for connection with Unity):

`c:\opt\ros\foxy\x64\setup.bat`  Source the setup file

`cd C:\[Path To The Repo]\ros2_emulation\src`

`call install\setup.bat` Source the workspace

`ros2 run ros_tcp_endpoint default_server_endpoint --ros-args -p ROS_IP:='your ip address' -p ROS_TCP_PORT:=10000` Use `ipconfig` to find your IPv4 address for ROS_IP.

Open Unity:

1. Go to the ROS Settings and change the corresponding ROS IP address there. You can follow the official instruction [here](https://github.com/Unity-Technologies/Unity-Robotics-Hub/blob/main/tutorials/ros_unity_integration/setup.md#-unity-setup), just do Step 3.

2. Open the game scene and hit play. Then rerun the player_detection node — you will see players' blobs shown in the Unity editor.

Note: This part is only for development use. In regular procedure, we have startup scripts that run this part automatically onsite.

### One Button Start-Up

The startup scripts are located in [this](https://drive.google.com/drive/folders/1dst3uxRbtqcubARKFlGByCg3i5g69zFd?usp=drive_link) Google Drive shared folder.

**run_system.bat** — Windows Batch startup script for running the system locally. Set the environment variables under "===== SET ENVIRONMENT VARIABLES =====" for your local system. The script finds the ros2_emulation directory automatically as long as the batch script is in the same directory.

**run_main_system.bat** — Startup script originally made for the Packard facility demo. *(Packard's Field no longer exists — kept for reference only.)*

**Local startup procedure:**
1. Run the Unity game. Ensure the IP address has been configured in the ROS2 settings in Unity.
2. Double-click the startup script. The system should now be running.

**Packard facility procedure:** *(outdated — Packard's Field no longer exists)*
1. Run the Unity game executable (build the game) on the rightmost computer.
2. Double-click the startup script on the leftmost computer.
3. Minimize all terminal windows except for the VITE server.
4. Enter the third link from the VITE server terminal into your phone's browser to access the web app.

Extra notes:
- The script's auto-detect IP method doesn't always work — set the IP address manually if needed (`ipconfig` → IPv4 address).
- The local script rebuilds packages every time. The Packard script does not — rebuild manually after changes to ros2_emulation. *(Packard's Field no longer exists.)*

### Folder Structure

Our folder structure follows the standard [ROS2 workspace](https://docs.ros.org/en/foxy/Tutorials/Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.html) structure.

```
ros2_emulation/
└── src/
    ├── ap_interfaces/           # Core CV and hardware package
    │   ├── src/
    │   │   ├── player_detection.cpp   # Multi-threaded CV player tracking
    │   │   └── button.cpp             # BLE button input handler
    │   ├── msg/
    │   │   ├── Pos.msg                # Player position message
    │   │   ├── Button.msg             # Button state message
    │   │   └── Score.msg              # Score message
    │   ├── srv/
    │   │   └── AddThreeInts.srv       # Game parameter service
    │   ├── launch/
    │   │   └── airplay_launch.py      # Launches detection + button nodes
    │   └── CMakeLists.txt             # Build config (OpenCV, Spinnaker, SimpleBLE, Eigen)
    ├── ROS-TCP-Endpoint-main-ros2/    # TCP bridge (v0.7.0)
    │   ├── ros_tcp_endpoint/
    │   │   ├── server.py              # TCP server core
    │   │   ├── tcp_sender.py          # Outbound TCP message queue
    │   │   ├── publisher.py           # ROS → Unity message forwarding
    │   │   ├── subscriber.py          # ROS topic → Unity forwarding
    │   │   ├── service.py             # ROS service client from Unity
    │   │   └── unity_service.py       # ROS service server in Unity
    │   └── launch/
    │       └── endpoint.py            # Launches TCP endpoint on :10000
    ├── unity_robotics_demo/           # Demo/testing nodes
    └── unity_robotics_demo_msgs/      # Demo message definitions
```

### Dependencies

In ROS2, dependencies are declared in CMakeLists.txt and package.xml.

For built-in packages like `rclcpp`, `std_msgs`: add `find_package(package REQUIRED)` in CMakeLists.txt and `<depend>package</depend>` in package.xml.

For external packages:
- **Spinnaker** (FLIR camera): Custom CMake finder at `ap_interfaces/cmake/FindSpinnaker.cmake`. **If using a FLIR camera on a new PC, update the hardcoded path in FindSpinnaker.cmake.**
- **SimpleBLE** (kick buttons): Linked via CMake commands. See CMakeLists.txt.
- **OpenCV 4.11.0**: Standard `find_package(OpenCV 4.11.0 REQUIRED)`
- **Eigen 3.4.0**: Used for Kalman filter matrix operations. Path is hardcoded in CMakeLists.txt.

Follow the [ament CMake documentation](https://docs.ros.org/en/foxy/How-To-Guides/Ament-CMake-Documentation.html?highlight=library) for adding new dependencies.

## ROS2-Unity Bridge

We use the [Unity Robotics Hub](https://github.com/Unity-Technologies/Unity-Robotics-Hub/tree/main) TCP endpoint for the connection between ROS2 and Unity.

### How It Works

The `ros_tcp_endpoint` runs a TCP server on port 10000 that bridges ROS2 topics/services to Unity:

1. Unity connects to the TCP server
2. Unity registers topic subscriptions via system commands (e.g., `__subscribe`)
3. When a ROS2 node publishes to a subscribed topic, the TCP bridge serializes the message and sends it to Unity
4. Unity can also call ROS2 services or publish to ROS2 topics through the same TCP connection

**Message Format**: `[dest_length][destination][message_length][serialized_message]`

**Protocol Version**: v0.7.0 (handshake on connect)

### Key Scripts

* [player_detection.cpp](/posts/player-detection-script/)
* [button.cpp](/posts/button-script/)

### Latency Tests

* **pub_diff**: Time difference between consecutive publishing messages. File found in the first src folder.
* **detect_duration**: Latency for the player detection pipeline. File found in the first src folder.
* The implementation of these latency tests can be found in [player_detection.cpp](/posts/player-detection-script/).

## Contact
helloigym@umich.edu
