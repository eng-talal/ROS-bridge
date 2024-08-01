# ROS1 Noetic to ROS2 Foxy Bridge

This repository provides a guide and scripts to set up a bridge between ROS1 Noetic and ROS2 Foxy, enabling seamless communication between nodes running on different ROS distributions. This bridge is essential for projects that require integration between legacy ROS1 systems and newer ROS2 systems.

## Prerequisites

Before setting up the bridge, ensure that you have the following prerequisites installed on your system:

- **ROS1 Noetic**: [Installation Guide](http://wiki.ros.org/noetic/Installation)
- **ROS2 Foxy**: [Installation Guide](https://docs.ros.org/en/foxy/Installation.html)
- **Ubuntu 20.04**: The recommended operating system for compatibility with both ROS1 Noetic and ROS2 Foxy.

### Additional Dependencies

Install the following additional dependencies:

```bash
sudo apt update
sudo apt install -y python3-vcstool python3-colcon-common-extensions
```

## Installation
### Step 1: Setting Up Workspaces
Create separate workspaces for ROS1 and ROS2.

to create workspace for ROS1 open the terminal and run the following comands :
```bash
mkdir -p ~/ros1_ws/src
cd ~/ros1_ws
source /opt/ros/noetic/setup.bash
catkin_make
```

to create workspace for ROS2 open **another terminal** "this is necessary to avoid errors" because you can't run the source command for different ROS distributions in the same terminal 

```bash
mkdir -p ~/ros2_bridge_ws/src
cd ~/ros2_bridge_ws
source /opt/ros/foxy/setup.bash
colcon build
```
 After creating workspaces you will find two Directories or workspaces for ROS1 and ROS2 in ```home``` path

<div align="center">
    <img src="https://github.com/user-attachments/assets/d9d868b2-f49a-4a8a-9536-708f3dfef5b2" width="600">
</div>

### Step 2: install bridge package
open the terminal and write the following command to install ROS bridge package :
```bash
sudo apt install ros-foxy-ros1-bridge
```
- **note:** the bridge package is ROS2 package not ROS1 package  

<div align="center">
    <img src="https://github.com/user-attachments/assets/8455f9d4-dad2-47dc-91a2-149de931139d" alt="Screenshot" width="600">
</div>

## Running the Bridge 

### Step 1: Source the Setup Files
Source the setup files for both ROS1 and ROS2 in a new terminal.

```bash
# Source ROS1
source /opt/ros/noetic/setup.bash
# Source ROS2
source /opt/ros/foxy/setup.bash
```

<img src="https://github.com/user-attachments/assets/3397f66a-5992-44dc-b16b-50bcf792e24b" alt="Screenshot" width="600">  


- It is not correct to Source setup for different ROS distributions in the same terminal but this is an exception for the bridge to work.

### Step 2: Start the Bridge
Run the ROS1-ROS2 bridge.

```bash
ros2 run ros1_bridge dynamic_bridge
```

<img src="https://github.com/user-attachments/assets/39ce85e0-ef25-440b-a990-74601f5ef350" alt="Screenshot" width="600">  

* **Error** here because there is no master node in ROS1 and if you run ```roscore``` on another terminal the bridge is working.

## Verification

To verify the bridge is working, you can test the communication between ROS1 and ROS2 nodes.

### Test with ROS1 Publisher and ROS2 Subscriber
after [Running the Bridge] (#Running the Bridge) in the previous steps
  

#### 1. ROS1 Publisher:
open the terminal and start the ROS core using ```roscore``` :

```bash
source /opt/ros/noetic/setup.bash
roscore
```

In a new terminal:

```bash
source /opt/ros/noetic/setup.bash
rostopic pub /chatter std_msgs/String "data: 'Hello from ROS1'"
```

#### 2. ROS2 Subscriber:
In another terminal, subscribe to the topic:

```bash
source /opt/ros/foxy/setup.bash
ros2 topic echo /chatter
```

You should see the message from ROS1 appearing in the ROS2 terminal.

-

