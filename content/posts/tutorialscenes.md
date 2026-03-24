---
title: "Tutorial Scenes"
---
There are multiple scenes that have been created to guide first-time users of the system. Each scene corresponds to their own keyboard input so the moderator can freely skip between them as they see fit.

* **Blank** — An empty scene used to activate and adjust the cameras so that the game field is accurately displayed. Keyboard input: **0**
* **Easy** — Shows the speed and size of the ball for the easy mode. Keyboard input: **1**
* **Medium** — Shows the speed and size of the ball for the medium mode. Keyboard input: **2**
* **Hard** — Shows the speed and size of the ball for the hard mode. Keyboard input: **3**
* **Score** — Shows the player what it is like to use their peripersonal boundary to 'kick' the ball and score a point. Keyboard input: **4**
* **Teams** — Used for the moderator to split the players onto two sides of the field. Keyboard input: **5**
* **Load** — Loads the player detection script and ROS2-Unity bridge connection script. Prepares the game for interaction with physical players. Keyboard input: **6**
* **Win** — Shows the end state of the game with the winning side receiving a crown. Keyboard input: **7**

Note: In the networked version, the game mode (Easy/Medium/Hard) is synced via the `GameManager.syncedMode` NetworkVariable from the server. The tutorial scenes still use the legacy keyboard-driven mode selection for local preview purposes.
