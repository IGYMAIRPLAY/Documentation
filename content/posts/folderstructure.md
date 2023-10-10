---
title: "Scripts"
---
# Folder Structure
* **Air Hockey**
    * **Goal Control** - plays victorious goal sound effect if hockey hits either of the two goals
    * **Wall Control** - plays boundary sound effect if hockey hits the wall
    * **Hockey Ctr** - if debug is checked in the inspector, can use keyboard input to move hockey and test game mechanics (for playtesting on PC only)
    * **Instructions** - loads tutorial scene corresponding to number keys [0-8] (for playtesting only)
    * **Game Manager** - handles game rules including scores, winning / losing, timer
* **Shapes Catcher**
    * **Emitter** - spawns random shapes at a specified time interval in random directions to both sides of the field
    * **Shape Class** - defines the three different shapes (Circle, Square, Triangle) 
    * **Shape Behavior** - shape disappears after specified time passes or collides with player and handles player score
    * **Switch Shape** - uses keyboard input for switching shape (for playtesting only)
    * **Timer** - handles timer and game logic when timer stops
    * **Shapes Game Manager** - handles game rules including scores, winning / losing, timer
    * **Pos Message SC** - separate posmessage script for shapes catcher game, may need to change later so there is better architecture; when player presses the bluetooth button, only the boundary of the player on the left field switches shape
* **Camera Adjust** - calibrates camera position
* **Pos Message** - reads player position input and draws a peripersonal boundary around players based on radius and position (currently only specific to air hockey game); when player presses the bluetooth button, only the boundary of the player on the left field expands to a specified radius
* **Start Up** - runs player detection code and starts up ROS connection before loading the game scene
    * [Link to Reference](https://stackoverflow.com/questions/1469764/run-command-prompt-commands)