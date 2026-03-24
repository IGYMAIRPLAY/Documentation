---
title: "Prefabs"
---
## Air Hockey

* **Ball (Circle1)**: The ball for the Air Hockey game. Contains hockeyctr (ball controller), HockeyNetworkTransform (network position sync), and Rigidbody2D (server-authoritative physics). This is a NetworkObject owned by the server.
* **Paddle (CircleController)**: Player paddle prefab. A client-owned NetworkObject with CircleController for collision/hit detection, NetworkTransform for position sync, and display name NetworkVariable.
* **Field**: The playing field for the Air Hockey game.

## Shapes Catcher
* Circle_Shape: The circle shaped GameObject for the Shapes Catcher game. Grants the player 1 point.
* Square_Shape: The square shaped GameObject for the Shapes Catcher game. Grants the player 1 point.
* Triangle_Shape: The triangle shaped GameObject for the Shapes Catcher game. Grants the player 1 point.
* Heart_Shape: The heart shaped GameObject for the Shapes Catcher game. Grants the player 5 points.
