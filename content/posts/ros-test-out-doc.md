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
* **table-demo**: This branch is used for our table-demo at Roland's Office. There is no button logic for the table-demo branch.

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

## Contact 
helloigym@umich.edu