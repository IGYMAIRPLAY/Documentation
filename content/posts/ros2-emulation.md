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

[Here]([Windows (binary) — ROS 2 Documentation: Foxy documentation](https://docs.ros.org/en/foxy/Installation/Windows-Install-Binary.html)) is the installation for ROS2 foxy official document (You might encounter some errors later while building the workspace)

[Here](https://ms-iot.github.io/ROSOnWindows/GettingStarted/SetupRos2.html) is an alternative way to install (only in disk C) (recommended)

### Button

## Overview

The system overview is written in [here](https://docs.google.com/presentation/d/1leWxa7IAV4V9gBXjTLt8Q2WAHMvET5B8hrSyY8mlWHI/edit?usp=drive_link). The numbers at the end means the latest modification date. 

## Development

### Branches

* **OnsiteVersionCuda_Multithreads** (Currently using): This branch is for testing the system **WITH GPU** and Multithread for Grabbing frames on-site at Packard's Field.
* **OnsiteVersionCuda**: This branch is for testing the system **WITH GPU** on-site at Packard's Field.
* **OnsiteVersion**: This branch is for testing the system **WITH CPU** on-site at Packard's Field.
* **video_test**: This branch is for testing the system with the video recordings taken from on-site **WITH CPU**. Due to the video's size exceeding the maximum size to push to GitHub, make sure to download the video from this [link](https://drive.google.com/file/d/1uPAa-w7rQWaxT7_s6YWcZ-Tj5pzy42DI/view?usp=drive_link), and store it in the /ap_interfaces/test_video/ directory before running the program. There is no button logic for all video test branches.
* **video_testGPU**: This branch is for testing the system with the video recordings taken from on-site **WITH GPU**. Before running the procedure, please install OpenCV_CUDA 4.9 within this [link](https://drive.google.com/file/d/1fTxBHLDgS5kJSOQfCRs_7UuPasXRqUHW/view?usp=drive_link).It would take at least 1 hour to complete the installation. Due to the video's size exceeding the maximum size to push to GitHub, make sure to download the video from this [link](https://drive.google.com/file/d/1uPAa-w7rQWaxT7_s6YWcZ-Tj5pzy42DI/view?usp=drive_link), and store it in the /ap_interfaces/test_video/ directory before running the program. There is no button logic for all video test branches.
* **OnsiteVersionCuda_Multithreads_UWB** (Testing branch for UWB): this branch is used for testing UWB localization. Please refer to the [UWB repo](https://github.com/IGYMAIRPLAY/UWB) for details.
* **table-demo**(Currently unavailable since we don't have two computers to deal with the latency issue for table demo): This branch is for testing the system on our table demo. There is no button logic for the table-demo branch.

### Running procedure

Open a terminal:

`c:\opt\ros\foxy\x64\setup.bat`  Source the setup file

`cd C:\[Path To The Repo]\ros2_emulation\src` 

`colcon build --merge-install --packages-select ap_interfaces` only build ap_interfaces or if you want to build all: `colcon build --merge-install`Everytime you made a change you need to build it.

`call install\setup.bat` Source the workspace

`ros2 launch ap_interfaces airplay_launch.py` to run the button and player_detection node or `ros2 run ap_interfaces player_detection` to run merely the player_detection node

Open another terminal (for connection with Unity):

`c:\opt\ros\foxy\x64\setup.bat`  Source the setup file

`cd C:\[Path To The Repo]\ros2_emulation\src` 

`call install\setup.bat` Source the workspace

`ros2 run ros_tcp_endpoint default_server_endpoint --ros-args -p ROS_IP:='your ip address' -p ROS_TCP_PORT:=10000` You can use ipconfig to check the available ip address you can choose. Then copy the ipv4 address into the ROS_IP.

Open Unity Repo:

1, Go to the ROS Settings and change the corresponding ROS IP address there. You can follow the official instruction [here](https://github.com/Unity-Technologies/Unity-Robotics-Hub/blob/main/tutorials/ros_unity_integration/setup.md#-unity-setup), just do Step 3.

2，Open the 'Airplay' scene and hit running. Then rerun the player_detection node, you will see players' blob shown on the unity editor.



Note:

This part is only for development use. In regular procedure, we have start-up script that help us run this part automatically onsite.

### One Button Start-Up

The startup scripts are located in [this](https://drive.google.com/drive/folders/1dst3uxRbtqcubARKFlGByCg3i5g69zFd?usp=drive_link) google drive shared folder.

run_system.bat - this is a Windows Batch startup script that can be used when running the system locally on your laptop. Make sure to set the environment variables under "===== SET ENVIRONMENT VARIABLES =====" for your local system. The script can find the ros2_emulation directory automatically as long as this batch script is kept in the same directory that the ros2_emulation directory is located.

run_main_system.bat - this is the startup script that is used to demo the system at the Packard facility. 

The startup Batch scripts runs all the commands needed to to get the system running. Any line that starts with "REM" is a comment.

One button startup procedure on local system:
1. Run the Unity game (ros-test-out). Ensure that the ip-address has been configured in the ROS2 settings in Unity.
2. Double-click the startup script. Now, the system should be running.

One button startup procedure at Packard facility:
1. Run the Unity game executable (i.e. build the game) on the rightmost computer.
2. Double-click the startup script on the leftmost computer.
3. Minimize all the terminal winsows that pop up on the leftmost computer except for the VITE server.
4. Enter the third link on the VITE server terminal into your phone's browser to access the web app to control the system.

Extra notes:
- the script's find ip address method under "==== Get IP Address ====" doesn't always work, so please set the ip address manually if it doesn't work (ip address can be found by running ipconfig and then pick the ipv4 address).
- the local script (run_system.bat) rebuilds the packages everytime it is run. The demo script for the Packard facility (run_main_system.bat) does not rebuild the packages, so if changes are made to ros2_emulation, make sure to rebuild!

### Folder structure

Our folder structure follows the structure of standard [ROS2 workspace](https://docs.ros.org/en/foxy/Tutorials/Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.html) 

![ROS2 Folder structure](/ROS2FolderStructure.png)

### Add dependencies

In ROS2, we usually add dependencies in CmakeLists.txt and Package.xml. In general, these two files tell what dependencies the package needs.

For some build-in package, like 'rclcpp', 'std_msgs',  we just simply add them by `find_package(package REQUIRED)` in CmakeLists.txt and `<depend>package</depend>` `  in Package.xml

For other non-build-in package, like 'Spinnaker' for FLIR camera and 'Simple-ble' for kick button. We have to manually add the dependencies with cmake command and link the library. You can follow the steps [here](https://docs.ros.org/en/foxy/How-To-Guides/Ament-CMake-Documentation.html?highlight=library). Note that for 'Spinnaker', this doesn't work so we used another way to add the dependency. You can check it in CmakeLists.txt as well as ap_interfaces/cmake/FindSpinnaker.cmake for details. (If you want to use the FLIR camera in a new PC, you need to **change the address in FindSpinnaer.cmake**. I hard coded this part.)

## ROS2-Unity Bridge

We use [this officially supported Repo](https://github.com/Unity-Technologies/Unity-Robotics-Hub/tree/main) for the connection between ROS2 and Unity part. 

### Key Scripts

* [player_detection.cpp]({{< ref "/player-detection-script" >}})

* [button.cpp]({{< ref "/button-script" >}})

### Latency Tests

* **pub_diff**: We measured the time difference between every two publishing messages. This file can be found in the first src folder.
* **detect_duration**: We measured the latency for player detection code. This file can be found in the first src folder.
* The implementation of these latency tests can be found in [player_detection.cpp]({{< ref "/player-detection-script" >}}).

## Contact 
helloigym@umich.edu
