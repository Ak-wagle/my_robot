## Robot Package Template

This is a GitHub template. You can make your own copy by clicking the green "Use this template" button.

It is recommended that you keep the repo/package name the same, but if you do change it, ensure you do a "Find all" using your IDE (or the built-in GitHub IDE by hitting the `.` key) and rename all instances of `my_robot` to whatever your project's name is.

Note that each directory currently has at least one file in it to ensure that git tracks the files (and, consequently, that a fresh clone has direcctories present for CMake to find). These example files can be removed if required (and the directories can be removed if `CMakeLists.txt` is adjusted accordingly).# my_robot


# my_bot — ROS2 Differential Drive Robot

A ROS2 (Humble) based differential drive robot with simulation, SLAM, navigation, LiDAR, camera, and ball tracking support.

---

## Prerequisites

### 1. ROS2 Humble
Make sure ROS2 Humble is installed on your system. If not, follow the official guide:
https://docs.ros.org/en/humble/Installation.html

### 2. Install Ignition Fortress (Gazebo)

First, install the required tools:
```bash
sudo apt-get update
sudo apt-get install lsb-release gnupg
```

Add the OSRF apt repository and install Ignition Fortress:
```bash
sudo curl https://packages.osrfoundation.org/gazebo.gpg --output /usr/share/keyrings/pkgs-osrf-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/pkgs-osrf-archive-keyring.gpg] https://packages.osrfoundation.org/gazebo/ubuntu-stable $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/gazebo-stable.list > /dev/null

sudo apt-get update
sudo apt-get install ignition-fortress
```

You can verify the installation by running:
```bash
ign gazebo
```

---

## Installation

### 1. Clone the repository

Navigate to your ROS2 workspace `src` folder and clone the repo:
```bash
cd ~/ros2_ws/src
git clone https://github.com/Ak-wagle/my_robot.git
```

### 2. Install all ROS2 dependencies

From the workspace root, run rosdep to install all required packages automatically:
```bash
cd ~/ros2_ws
rosdep update
rosdep install --from-paths src --ignore-src -r -y
```

### 3. Build the workspace

```bash
cd ~/ros2_ws
colcon build --symlink-install
```

### 4. Source the workspace

```bash
source ~/ros2_ws/install/setup.bash
```

> Tip: Add this line to your `~/.bashrc` so you don't have to run it every session:
> ```bash
> echo "source ~/ros2_ws/install/setup.bash" >> ~/.bashrc
> ```

---

## Usage

### Launch Simulation (Gazebo)
```bash
ros2 launch my_bot launch_sim.launch.py
```

### Launch on Real Robot
```bash
ros2 launch my_bot launch_robot.launch.py
```

### SLAM (Mapping)
```bash
ros2 launch my_bot online_async_launch.py
```

### Localization (AMCL)
```bash
ros2 launch my_bot localization_launch.py
```

### Navigation (Nav2)
```bash
ros2 launch my_bot navigation_launch.py
```

### Joystick Control
```bash
ros2 launch my_bot joystick.launch.py
```

### Ball Tracker
```bash
ros2 launch my_bot ball_tracker.launch.py
```

---

## Package Structure

```
my_bot/
├── launch/          # All launch files
├── config/          # YAML parameter files
├── description/     # URDF / xacro robot model
├── worlds/          # Gazebo world files
├── maps/            # Pre-built maps for localization
└── package.xml      # Package dependencies
```

---

## Dependencies

All dependencies are declared in `package.xml` and will be installed automatically by `rosdep`. Key packages include:

- `ros2_control` / `ros2_controllers` — hardware abstraction and controller management
- `ign_ros2_control` — Ignition Gazebo ↔ ros2_control bridge
- `slam_toolbox` — online SLAM
- `nav2_bringup` — full Nav2 navigation stack
- `slam_toolbox` — SLAM mapping
- `rplidar_ros` — RPLidar driver
- `v4l2_camera` — USB camera driver
- `twist_mux` — velocity command multiplexer
- `ball_tracker` — vision-based ball tracking

---

## Troubleshooting

**Controller spawners waiting forever (`Could not contact service /controller_manager/list_controllers`)**

This usually means `ign_ros2_control` is not installed. Run:
```bash
sudo apt install ros-humble-ign-ros2-control
```

**Gazebo packages not found during rosdep**

Make sure the OSRF apt repository is added (Step 2 above) before running `rosdep install`.
