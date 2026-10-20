# One-Click ROS 2 Startup in Windows Terminal with Pixi

A guide to automating the startup and environment initialization for ROS 2 inside Pixi environments on Windows using a dedicated Windows Terminal profile.

## The Problem
Running ROS 2 on Windows often requires navigating to your workspace, activating the Pixi environment (`pixi shell`), and sourcing the ROS 2 setup batch script (`call local_setup.bat`). Because `pixi shell` launches a nested interactive subshell, standard terminal chaining commands like `cmd /k` can drop out of scope or fail to retain ROS 2 environment variables.

---

## Solution

### 1. Create the Launch Script
Create a helper script inside your workspace (`C:\pixi_ws\launch_ros2.bat`):

```bat
@echo off

:: Sourcing the ROS 2 environment
cd /d C:\pixi_ws
call C:\pixi_ws\ros2-windows\local_setup.bat
cmd /k
```

### 2. Configure Windows Terminal
1. Open Windows Terminal and press `Ctrl + ,` to open Settings.
2. Click **Open JSON file** at the bottom-left.
3. In the `"profiles": { "list": [ ... ] }` array, add:

```json
{
  "guid": "{b2c4e567-89ab-cdef-0123-456789abcdef}",
  "name": "ROS 2 (Pixi)",
  "commandline": "cmd.exe /k \"cd /d C:\\pixi_ws && pixi run -- cmd /k C:\\pixi_ws\\launch_ros2.bat\"",
  "startingDirectory": "C:\\pixi_ws",
  "hidden": false
}
```

---

## Verification
Open your new tab from the Windows Terminal dropdown and verify:

```cmd
ros2 --help
ros2 run demo_nodes_cpp talker
```
