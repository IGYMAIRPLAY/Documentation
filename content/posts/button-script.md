---
title: "Button Script"
---
> **Note:** The kick button system has not been used in a long time and is untested with the current networking architecture. This documentation is preserved for reference but the feature should be considered unsupported until verified.

## button.cpp

The button node handles Bluetooth Low Energy (BLE) wireless kick controller input for the iGYM system.

### How It Works

The node scans for and connects to SimpleBLE devices (2 BLE clickers identified by MAC address). It reads button press state from a specific BLE characteristic and publishes the state to the `kick_size` topic.

### Multi-Threaded Architecture

| Thread | Purpose |
|--------|---------|
| **Kick Control Thread** | Reads button state from BLE devices every 10ms |
| **Publisher Thread** | Publishes button states at ~60Hz (16.67ms timer) |

### BLE Configuration

- **Library**: SimpleBLE
- **BLE Characteristic UUID**: `beb5483e-36e1-4688-b7f5-ea07361b26a8`
- **Devices**: 2 BLE clickers connected by MAC address
- Players must press the button on startup of the game for successful BLE connection

### Published Topic

**`kick_size`** (message type: `ap_interfaces/Button`):
- `kick[32]` — Boolean array of button states for up to 32 players

### Subscribed Topic

**`control`** (`std_msgs/String`): Control messages for configuring the button node at runtime.
