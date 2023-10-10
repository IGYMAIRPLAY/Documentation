---
title: "ros2-emulation Documentation"
---
## Introduction 

Welcome to the ros2-emulation wiki! ros2-emulation is the repository for the ROS2 part of the entire [iGYM system](https://www.igym.solutions/). iGYM aims to build games that are inclusive and fun for people of all abilities. We use projectors to display a virtual game field for players to physically interact with. Through the implementation of peripersonal boundaries and kick buttons, we level the playing field between people with motor disabilities and their non-disabled peers.

The code in this repository is responsible for implementing the player detection in the iGYM system using OpenCV.

The current most up-to-date branch that is used for the onsite play field is the OnsiteVersion branch.

This section is still in progress...

## Requirements
* Windows 10
* ROS2 foxy

## Installation

### ROS2 foxy

### Button

## Overview

Player detection:

Kick Button: 

## Development

### Branches

* **0309**: This branch is for testing the system on-site at Packard's Field.
* **3_09_video_test**: This branch is for testing the system with the video recordings taken from on-site. Please make sure the file path to the video is adjusted correctly.
* **table-demo**: This branch is for testing the system on our table demo. There is no button logic for the table-demo branch.

### Running procedure

### Folder structure

Our folder structure follows the structure of standard [ROS2 workspace](https://docs.ros.org/en/foxy/Tutorials/Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.html) 

![ROS2 Folder structure](/ROS2FolderStructure.png)

### Add dependencies

### Key Scripts

* [player_detection.cpp]({{< ref "/player-detection-script" >}})

* [button.cpp]({{< ref "/button-script" >}})

  

### Latency Tests

* **pub_diff**: We measured the time difference between every two publishing messages. This file can be found in the first src folder.
* **detect_duration**: We measured the latency for player detection code. This file can be found in the first src folder.
* The implementation of these latency tests can be found in [player_detection.cpp]({{< ref "/player-detection-script" >}}).

## Contact 
helloigym@umich.edu