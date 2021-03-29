# AWS DeepRacer Traffic Simulation Traffic Sign Package

## Overview

The Traffic Simulation Traffic Sign ROS package creates the traffic_sign_node which interprets traffic lights and signs detected by the object_detection_node. For more information about the Traffic Simulation project, see [Traffic Simulation project](https://github.com/jochem725/aws-deepracer-traffic-sim).

## License

The source code is released under [Apache 2.0](https://aws.amazon.com/apache-2-0/).

## Installation

### Prerequisites

The Traffic Simulation project is a sample application built on top of the existing AWS DeepRacer application uses object detection machine learning model through which the AWS DeepRacer device can identify and interpret traffic signs. For detailed information on Traffic Simulation project, see Traffic Simulation project [Getting Started](https://github.com/jochem725/aws-deepracer-traffic-sim/blob/main/getting-started.md) section.

The traffic_sign_pkg specifically depends on the following ROS2 packages as build and execute dependencies:

1. *deepracer_interfaces_pkg* - This packages contains the custom message and service type definitions used across the AWS DeepRacer core application, modified to support Traffic Simulation project.

### Downloading and Building

Open up a terminal on the DeepRacer device and run the following commands as root user.

1. Switch to root user before you source the ROS2 installation:

        sudo su

1. Source the ROS2 Foxy setup bash script:

        source /opt/ros/foxy/setup.bash 

1. Set the environment variables required to run Intel OpenVino scripts:

        source /opt/intel/openvino_2021/bin/setupvars.sh

1. Create a workspace directory for the package:

        mkdir -p ~/deepracer_ws
        cd ~/deepracer_ws

2. Clone the entire Traffic Simulation project on the DeepRacer device.

        git clone https://github.com/jochem725/aws-deepracer-traffic-sim.git
        cd ~/deepracer_ws/aws-deepracer-traffic-sim/deepracer_traffic_sim_ws/

3. Fetch unreleased dependencies:

        cd ~/deepracer_ws/aws-deepracer-traffic-sim/deepracer_traffic_sim_ws/
        rosws update

4. Resolve the dependencies:

        cd ~/deepracer_ws/aws-deepracer-traffic-sim/deepracer_traffic_sim_ws/ && apt-get update
        rosdep install -i --from-path . --rosdistro foxy -y

5. Build the traffic_sign_pkg and deepracer_interfaces_pkg:

        cd ~/deepracer_ws/aws-deepracer-traffic-sim/deepracer_traffic_sim_ws/ && colcon build --packages-select traffic_sign_pkg deepracer_interfaces_pkg


## Usage

Although the **traffic_sign_node** is built to work with the Traffic Simulation project, it can be run independently for development/testing/debugging purposes.

### Run the node

To launch the built traffic_sign_node as root user on the DeepRacer device open up another terminal on the DeepRacer device and run the following commands as root user:

1. Switch to root user before you source the ROS2 installation:

        sudo su

2. Navigate to the Traffic Simulation workspace:

        cd ~/deepracer_ws/aws-deepracer-traffic-sim/deepracer_traffic_sim_ws/

3. Source the ROS2 Foxy setup bash script:

        source /opt/ros/foxy/setup.bash 

4. Source the setup script for the installed packages:

        source ~/deepracer_ws/aws-deepracer-traffic-sim/deepracer_traffic_sim_ws/install/setup.bash 

5. Launch the traffic_sign_node using the launch script:

        ros2 launch traffic_sign_pkg traffic_sign_pkg_launch.py

## Launch Files

A launch file called traffic_sign_pkg_launch.py is included in this package that gives an example of how to launch the traffic_sign_node.

        from launch import LaunchDescription
        from launch_ros.actions import Node

        def generate_launch_description():
            return LaunchDescription([
                Node(
                    package='traffic_sign_pkg',
                    namespace='traffic_sign_pkg',
                    executable='traffic_sign_node',
                    name='traffic_sign_node'
                )
            ])


## Node Details

### traffic_sign

#### Subscribed topics

| Topic Name | Message Type | Description |
|----------- | ------------ | ----------- |
|/object_detection_pkg/inference_results| InferResultsArray | Message with detected objects as InferResults and the image for which they were detected |

#### Published Topics

| Topic Name | Message Type | Description |
| ---------- | ------------ | ----------- |
|traffic_sign_results|TrafficSign|Message with detected traffic signs that can be used as input for navigation actions.|

## Resources

* AWS DeepRacer Opensource getting started: [https://github.com/awsdeepracer/aws-deepracer-launcher/blob/main/getting-started.md](https://github.com/awsdeepracer/aws-deepracer-launcher/blob/main/getting-started.md)
* Traffic Simulation project getting started: [https://github.com/jochem725/aws-deepracer-traffic-sim/blob/main/getting-started.md](https://github.com/jochem725/aws-deepracer-traffic-sim/blob/main/getting-started.md)