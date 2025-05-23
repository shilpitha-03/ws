# ws Setup

### Install from source

#### ROS

Be sure you've installed
[ROS Humble](https://docs.ros.org/en/humble/Installation.html)
(at least ROS-Base). More ROS dependencies will be installed below.

#### Gazebo

Install either [Fortress, Harmonic or Ionic](https://gazebosim.org/docs). We need Harmonic:

    sudo apt update
    sudo apt install curl lsb-release gnupg

    sudo curl https://packages.osrfoundation.org/gazebo.gpg --output /usr/share/keyrings/pkgs-osrf-archive-keyring.gpg

    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/pkgs-osrf-archive-keyring.gpg] http://packages.osrfoundation.org/gazebo/ubuntu-stable $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/gazebo-stable.list > /dev/null
    
    sudo apt update
    sudo apt install gz-harmonic

Set the `GZ_VERSION` environment variable to the Gazebo version you'd
like to compile against. For example:

    export GZ_VERSION=harmonic # As we have decided to use harmonic

> You only need to set this variable when compiling, not when running.

#### Compile ros_gz

The following steps are for Linux and macOS.

1. Create a colcon workspace:

```
# Setup the workspace
mkdir -p Target-Tracker/new/ws/src
cd Target-Tracker/new/ws/src

# Download needed software
git clone https://github.com/gazebosim/ros_gz.git -b humble
```

1. Install dependencies (this may also install Gazebo):

```
cd Target-Tracker/new/ws
rosdep install -r --from-paths src -i -y --rosdistro humble
```

> If `rosdep` fails to install Gazebo libraries and you have not installed them before, please follow [Gazebo installation instructions](https://gazebosim.org/docs/latest/install). Or check what packages are needed from the CMakeLists.txt and sudo apt install <package> individually.

1. Build the workspace:

```
# Source ROS distro's setup.bash
source /opt/ros/humble/setup.bash

# Build and install into workspace
cd Target-Tracker/new/ws
colcon build
```