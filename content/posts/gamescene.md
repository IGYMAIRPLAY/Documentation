---
title: "Game Scene"
---
## Scene Objects
* Cameras 
    * Calibrates camera position
    * Left Camera - displays the left side of the field
    ![Left Camera](/gamescene_pic1.png)
    * Right Camera - displays the right side of the field
    ![Right Camera](/gamescene_pic2.png)
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