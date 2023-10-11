---
title: "Player Detection Code"
---
## player_detection.cpp

* **init_background() Function**: This function initializes the background image. It can either capture the initial frame as a background (when enable_flag is set to 1) or load a background image from a file.
* **background_subtraction() Function**: The background_subtraction() function takes the current frame and the background image as input and performs background subtraction. 
* **average_frame() Function**: This function computes the average of a sequence of frames.
* **detect_pos() Function**: This function handles object detection and tracking. It captures frames from a camera, performs background subtraction, and then identifies and tracks objects in the scene. 
* **Publisher Node (PublisherNode)**: The published data includes the number of detected objects, their positions (x and y coordinates), and sizes. Timing information is also included for performance evaluation.
* **Subscriber Node (SingleThreadedNode)**: This is a simple subscriber node that listens for control messages. (Currently is not used)