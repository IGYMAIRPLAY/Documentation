---
title: "Player Detection Code"
---
## player_detection.cpp

The player detection node is the core computer vision component that detects and tracks players on the physical game field. It uses background subtraction, contour detection, and Kalman filtering to produce real-time player positions.

### Multi-Threaded Architecture

The node uses three concurrent threads for maximum throughput:

| Thread | Purpose |
|--------|---------|
| **Capture Thread** | Reads frames from camera (FLIR Spinnaker or USB/OpenCV) |
| **Detection Thread** | Processes frames: background subtraction → contour detection → Kalman tracking |
| **Publisher Thread** | Publishes tracking results to `pos_raw` topic |

### Pipeline Functions

* **init_background()** — Initializes the background image. Can either capture the initial frame as background (when `enable_flag` is set to 1) or load a background image from a file.

* **background_subtraction()** — Takes the current frame and the background image as input and performs background subtraction to isolate foreground objects (players).

* **average_frame()** — Computes the average of a sequence of frames for noise reduction.

* **detect_pos()** — Main detection and tracking pipeline:
    1. Captures frames from camera
    2. Performs background subtraction
    3. Detects contours in the foreground mask
    4. **Kalman filtering** — Tracks each player with a 4D state vector (x, y, velocity_x, velocity_y) for smooth position estimation
    5. **Hungarian algorithm** — Associates new detections with existing player tracks using optimal assignment
    6. **Forward prediction** — Accounts for camera latency (~25ms) by predicting positions ahead

### Published Topic

**`pos_raw`** (message type: `ap_interfaces/Pos`):
- `total` — Number of detected players
- `timestamp` — Capture timestamp in ms
- `x[32]`, `y[32]` — Player positions
- `player_id[32]` — Unique player IDs maintained across frames
- `tag_id[32]` — RFID tag IDs (when available)
- `size[32]` — Player blob radius/size

### Subscribed Topic

**`control`** (`std_msgs/String`): Control messages for configuring the detection node at runtime. (Currently not actively used)

### Camera Support

- **FLIR Camera**: Uses Spinnaker SDK for high-performance industrial camera input
- **USB Camera**: Falls back to OpenCV VideoCapture for standard webcams

### Performance Logging

The node logs performance metrics to CSV files for latency analysis:
- Frame capture timing
- Detection processing duration
- Publishing intervals
