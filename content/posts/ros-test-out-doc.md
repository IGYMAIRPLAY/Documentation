---
title: "ros-test-out Documentation"
---
## Introduction 

Welcome to the ros-test-out wiki! ros-test-out is the repository for the Unity part of the entire [iGYM system](https://www.igym.solutions/). iGYM aims to build games that are inclusive and fun for people of all abilities. We use projectors to display a virtual game field for players to physically interact with. Through the implementation of peripersonal boundaries and kick buttons, we level the playing field between people with motor disabilities and their non-disabled peers.

Currently, we have two games that are implemented:
* [Air Hockey]({{< ref "/airhockey" >}})
* [Shape Catcher]({{< ref "/shapecatcher" >}})

## System Requirements
* Unity Editor 2020.3.16f1
* Windows 10

## Development
Branches
* **main**: This is the most up-to-date branch used for on-site testing at Packard's Field.
* **table-demo**(Currently, this branch is not working since we use two PC to address the latency issue): This branch is used for our table-demo at Roland's Office. There is no button logic for the table-demo branch.
* **UWBtest**: This branch is used for UWBtest only.

Game structure
* [Tutorial scenes]({{< ref "/tutorialscenes" >}})
* [Game scene]({{< ref "/gamescene" >}})

Folder structure
* [Materials]({{< ref "/materials" >}})
* [Prefabs]({{< ref "/prefabs" >}})
* [Resources]({{< ref "/resources" >}})
* [RosMessages]({{< ref "/rosmessages" >}})
* [Scenes]({{< ref "/scenes" >}})
* [Scripts]({{< ref "/folderstructure" >}})

Latency Tests
* **sub_diff**: We measured the time difference between every two subscribing messages.
* **rendering_time**: We measured the time difference between subscribing message and rendering the player boundary.
* The results of these two latency tests are printed to the console. To retrieve the .log file of the console log, go to **%USERPROFILE%\AppData\LocalLow\CompanyName\ProductName\Player.log**.
* The implementation of these latency tests can be found in [player_detection.cpp]({{< ref "/player-detection-script" >}}).

Tutorial Implementation
* **Interactable**: These are the objects that the user interacts with throughout the tutorial, like buttons or tasks
* **Interactable Object**: These are the GameObjects that hold onto an Interactable to be found by the Tutorial Manager and enabled/disabled as necessary
* **Scene Object**: These are the Scriptable Objects that contain all of the information about a particular 'scene'. They hold lists of all of the Interactables that need to be enabled or disabled, what the background image needs to be, and what the next 'scene' will be after a particular Interactable Object has been selected. 
* **Tutorial Manager**: This is the script that reads in all of the Interactable Objects and performs the operations outlined by the Scene Object. It does this via EventBus (Pub/Sub) events being published and methods being called via UnityEvents on the Interactable Objects.
* **Canvas Manager**: Currently just controls the setting of the image that the Tutorial Manager gets from the current Scene Object. This is done via EventBus with the Tutorial Manager publishing an UpdateImageEvent for the Canvas to respond to.

## Contact 
helloigym@umich.edu
