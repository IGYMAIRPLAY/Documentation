---
title: "ros2-emulation Documentation"
---
## Introduction 

Welcome to the ros2-emulation wiki! ros2-emulation is the repository for the ROS2 part of the entire [iGYM system](https://www.igym.solutions/). iGYM aims to build games that are inclusive and fun for people of all abilities. We use projectors to display a virtual game field for players to physically interact with. Through the implementation of peripersonal boundaries and kick buttons, we level the playing field between people with motor disabilities and their non-disabled peers.

The code in this repository is responsible for implementing the player detection in the iGYM system using OpenCV.

The current most up-to-date branch that is used for the onsite play field is the 0309 branch.

This section is still in progress...

## System Requirements
* Unity Editor 2020.3.16f1
* Windows 10

## Development
Branches
* **0309**: This branch is for testing the system on-site at Packard's Field.
* **3_09_video_test**: This branch is for testing the system with the video recordings taken from on-site. Please make sure the file path to the video is adjusted correctly.
* **table-demo**: This branch is for testing the system on our table demo. There is no button logic for the table-demo branch.

Scripts
* [player_detection.cpp]({{< ref "/player-detection-script" >}})
* [button.cpp]({{< ref "/button-script" >}})

Latency Tests
* **pub_diff**: We measured the time difference between every two publishing messages. This file can be found in the first src folder.
* **detect_duration**: We measured the latency for player detection code. This file can be found in the first src folder.
* The implementation of these latency tests can be found in [player_detection.cpp]({{< ref "/player-detection-script" >}}).

## Contact 
helloigym@umich.edu