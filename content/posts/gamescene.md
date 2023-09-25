---
title: "Game Scene"
---
## Scene Objects
* Cameras 
    * Calibrates camera position
    * Left Camera - displays the left side of the field
    <img width="500" alt="Left Camera View" src="https://github.com/helloigym/ros-test-out/assets/62974319/698d6c6a-b1e5-4395-9dd7-632050603ba9">

    * Right Camera - displays the right side of the field
    <img width="500" alt="Right Camera View" src="https://github.com/helloigym/ros-test-out/assets/62974319/69617848-6436-4960-b756-61567f2327cb">
* Controller 
    * Reads position and updates player peripersonal boundary 
    * Handles kick button logic 
* Field
    * Draws field lines using LineRenderer
* Canvas 
    * Displays UI including timer and score
* AudioManager
    * Handles audio logic including:
        * Boundary
        * Goal
        * Win 
        * Start
* GameManager
    * Handles game logic
    * Starts up Unity-ROS connection and runs player detection code 