# ROS Noetic Serial Chain Manipulator #

A guided workflow for designing & visualizing a 5-chain serial manipulator robot URDF, with EOL ROS Noetic, from an assigned kinematic diagram. The five-joint chain shown in this repository contains four revolute joints and one prismatic end-effector joint.

This repository uses selected documentation to cover the steps of going from a kinematic diagram interpretation, to DH Parameter Table analysis, to URDF geometry mapping, and validating a design with RViz visualization.

## Preview

[Serial Chain Manipulator Kinematic Diagram](docs/imgs/fiveJointSerialArm.png)


[Serial Chain Manipulator in RViz](docs/imgs/rvizSerialArm.png)

## Development Environment

- [Ubuntu 20.04 LTS](https://releases.ubuntu.com/focal/)
- [ROS 1 Noetic — Ubuntu Installation](https://wiki.ros.org/noetic/Installation/Ubuntu)
- Graphical URDF viewer like RViz

My development environment utilized Windows 11 Visual Studio Code and Ubuntu 20.04 through [WSL2](https://learn.microsoft.com/en-us/windows/wsl/install)

## Robot Structure

The URDF defines the following serial chain:

```text
root Link: base_link has 1 child(ren)
    child(1):  link1
        child(1):  link2
            child(1):  link3
                child(1):  link4
                    child(1):  link5
```

This shows that the model has six links (including `base_link`) and five joints.


## Design Approach
1.  Identifying types of joints & axes supplied in the diagram
2.  Interpret link dimensions & coordinate frames.
3.  Apply DH Parameter analysis concepts.
4.  Unit conversion from millimeters to meters and degrees to radians.
5.  Building a primitive geometry mapping for representing the physical linkages.
6.  Validating the model and joints in RViz.


## Download Source URDF Model
```bash
git clone https://github.com/nfry25/ROS-Noetic-Serial-Chain-Manipulator.git

cd ROS-Noetic-Serial-Chain-Manipulator
```

## Validate Source URDF 
```bash
source /opt/ros/noetic/setup.bash
sudo apt install libirdfoom-tools -y

check_urdf src/ros_noetic/urdf/manipulator.urdf
```
Valid if check prints the robot's link tree.

## Visualize

```bash
sudo apt install ros-noetic-robot-state-publisher -y
sudo apt install ros-noetic-joint-state-publisher -y
sudo apt install ros-noetic-rviz -y
```

In seperate terminals:

### 1: Start ROS

```bash
source /opt/ros/noetic/setup.bash

roscore
```

### 2: Load Model & Publish Transforms

In cloned repository:

```bash
source /opt/ros/noetic/setup.bash

rosparam set /robot_description -t \
  src/ros_noetic/urdf/manipulator.urdf

rosrun robot_state_publisher robot_state_publisher
```

### 3: Joint Sliders

```bash
source /opt/ros/noetic/setup.bash

rosrun joint_state_publisher_gui joint_state_publisher_gui
```

### 4: RViz

```bash
source /opt/ros/noetic/setup.bash

rosrun rviz rviz
```

### RViz Configuration

Ensure RViz configurations are set to the following to ensure the robot is able to be displayed.

| Setting | Value |  
|---|---|
| Global Options → Fixed Frame | `base_link` |
| Add Display | `RobotModel`  |
| RobotModel → **Robot Description**  | `robot_description`  |
| Add Display  | **TF**: Coordinate Frames |

For inspecting the joints the revolute joints and prismatic joint individually, utilize the joint slider GUI.


## Resources

Biggest learning resource, especially for DH Parameters, has been Addison Sears-Collins' Automatic Addison tutorials online. I highly recommend you visit his website and look at the provided tutorials by him:

- [How to find DH Parameter Tables](https://automaticaddison.com/how-to-find-denavit-hartenberg-parameter-tables/)
- [DH Review for Robotic Arms](https://automaticaddison.com/review-of-denavit-hartenberg-for-robotic-arms/)



## License

This project is licensed under the [Apache License 2.0](LICENSE).


## Author

Nicolas Frey