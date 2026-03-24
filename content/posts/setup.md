---
title: "iGym System Setup Guide (Windows)"
---

> Last updated: 19 November 2025. [View original Google Doc](https://docs.google.com/document/d/1TMLHn8tC8T075ESRzFUgJOSfceofNwoGGtoi9QKcri8/edit?tab=t.0)

## Quick Version Checklist

All versions must match exactly.

| Component | Version |
|-----------|---------|
| Unity (non-networked) | 2020.3.16f1 |
| Unity (networked) | 6000.0.53f1 |
| Python | 3.8.10 |
| OpenSSL | 1.1.1n (Win64 full installer) |
| ROS 2 | Foxy (binary for Windows) |
| OpenCV | 4.11.0 VC16 x64 |
| Spinnaker SDK | 4.2 |
| Visual Studio | 2022 Community + Desktop development with C++ |

---

## 1. Install Unity

Download [Unity Hub](https://unity.com/unity-hub) and install.

In Hub → Installs → Add, choose **2020.3.16f1** (required — get from the archive page).
For the networked version use **6000.0.53f1**.

Enable the following modules:
- Windows Build Support (IL2CPP)
- Visual Studio Editor

> **Networking note:** When you launch the networked project, all needed packages should auto-install.

---

## 2. Install Visual Studio 2022

Download [Visual Studio 2022 Community](https://visualstudio.microsoft.com/downloads/) and run the installer.

Select the following workloads:
- **Desktop development with C++** (MSVC v143, CMake tools)
- **Game development with Unity**
- **.NET desktop build tools**

After install, open **"x64 Native Tools Command Prompt for VS 2022"** to confirm it exists.

---

## 3. Install Teledyne Spinnaker SDK 4.2 *(if using FLIR cam)*

Download [Spinnaker SDK 4.2 Windows Full](https://www.teledynevisionsolutions.com/products/spinnaker-sdk/?model=Spinnaker%20SDK&vertical=machine%20vision&segment=iis) and run the installer.

Choose **Application Development** during setup.

Confirm `C:\Program Files\Teledyne\Spinnaker` exists — this will be `Spinnaker_DIR`.

---

## 4. Install ROS 2 Foxy + Dependencies

Follow the [official Windows installation guide](https://docs.ros.org/en/foxy/Installation/Windows-Install-Binary.html). The guide says Windows 10 only — ignore that, it works on Windows 11.

Key requirements:

- **Python 3.8.10** must be the first `python.exe` in your PATH. Any newer version will break `colcon` and `rclpy`.
- **OpenSSL 1.1.1n** (Win64 full installer) is required for Foxy's security checks. It is outdated and must be downloaded from the Internet Archive:
  [Win64OpenSSL-1_1_1n.exe](https://web.archive.org/web/20220316070710/https://slproweb.com/download/Win64OpenSSL-1_1_1n.exe)
- **Do not follow their OpenCV guide** — a newer version is installed separately in step 5.
- Install this specific ROS 2 Foxy release: [release-foxy-20230620](https://github.com/ros2/ros2/releases/tag/release-foxy-20230620)

Note the root of the ROS 2 install as `ROS2_ROOT`. The rest of the installation steps are straightforward as long as the above requirements are set correctly.

---

## 5. Install OpenCV 4.11.0

Download and install [OpenCV 4.11.0](https://opencv.org/releases/).

Note the install location — this will be `OpenCV_DIR`.

---

## 6. Build SimpleBLE (OpenBluetoothToolbox)

Some code still relies on this library.

Clone the repo:
```
https://github.com/simpleble/simpleble
```

From a folder like `C:\Desktop\iGym\simpleble\simpleble` (where the first `simpleble` is the repo root), run:

```bat
mkdir build
cd build
cmake .. -DCMAKE_INSTALL_PREFIX=../install -DSIMPLEBLE_BACKEND=Windows
cmake --build . --config Release --target install
```

Confirm `simpleble\install` exists — this will be `simpleble_DIR`.

---

## 7. Clone Project Repos

Clone the ROS2 repo:
```
https://github.com/IGYMAIRPLAY/ros2_emulation
```
The location of the highest `src` folder will be `REPO_PATH` (e.g. `...\ros2_emulation\src`).

Then clone the Unity project:
- **Non-networked:** `https://github.com/IGYMAIRPLAY/ros-test-out`
- **Networked:** `https://github.com/IGYMAIRPLAY/unity-networking`

---

## 8. Find Your IP Address

Run `ipconfig` and grab the IPv4 of **Ethernet adapter vEthernet** or your USB-Ethernet adapter. Paste it into `IP_ADDRESS` in the batch file.

> **Tip:** You can use `127.0.0.1` which is much simpler.

---

## 9. Build ros_tcp_endpoint

```bat
call C:\dev\ros2_foxy\setup.bat
cd C:\Users\<you>\...\ros2_emulation\src
colcon build --merge-install --packages-select ros_tcp_endpoint
```

If `colcon` is not installed:
```bat
pip install -U colcon-common-extensions
```

---

## 10. Configuration

Use this Python tool to help configure USB cameras. Save it somewhere accessible (e.g. `...\iGym\usb_cam_tool.py`):
[usb_cam_tool.py (Pastebin)](https://pastebin.com/8cAhj9UK)

You will need to manually update:
- Zoom values in `CameraOpenCV.cpp`
- Size values in `player_detection.cpp`

---

## 11. Create run_system.bat

Create a `.bat` script using the template below, updating environment variables based on what you noted during setup:
[run_system.bat template (Pastebin)](https://pastebin.com/4hmLu5qW)

### Python Version Troubleshooting

If you have issues with Python versions, run the following to verify and fix:

```bat
REM 1. Ensure Python 3.8 is active
python --version
REM Should show 3.8.x

REM 2. Reinstall correct empy version for ROS2 Foxy
pip uninstall -y empy
pip install empy==3.3.4

REM 3. Remove previous build artifacts
cd C:\Users\<you>\Desktop\iGym\ros2_emulation\src
rd /s /q build
rd /s /q install
rd /s /q log
```
