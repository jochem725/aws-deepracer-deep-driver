# AWS DeepRacer Traffic Simulation Navigation Package

## Overview

The Traffic Simulation Navigation ROS package creates the traffic_navigation_node which decides the logic based on traffic directives from traffic_sign_node. For more information about the Traffic Simulation  project, see [Traffic Simulation project](https://github.com/jochem725/aws-deepracer-traffic-sim).

## License

The source code is released under [Apache 2.0](https://aws.amazon.com/apache-2-0/).

## Installation

### Prerequisites

The Traffic Simulation project is a sample application built on top of the existing AWS DeepRacer application uses object detection machine learning model through which the AWS DeepRacer device can identify and interpret traffic signs. For detailed information on Traffic Simulation project, see Traffic Simulation project [Getting Started](https://github.com/jochem725/aws-deepracer-traffic-sim/blob/main/getting-started.md) section.

The traffic_navigation_pkg specifically depends on the following ROS2 packages as build and execute dependencies:

1. *deepracer_interfaces_pkg* - This packages contains the custom message and service type definitions used across the AWS DeepRacer core application, modified to support Traffic Simulation sample project.

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

1. Clone the entire Traffic Simulation sample project on the DeepRacer device.

        git clone https://github.com/jochem725/aws-deepracer-traffic-sim.git
        cd ~/deepracer_ws/aws-deepracer-traffic-sim/deepracer_traffic_sim_ws/

2. Fetch unreleased dependencies:

        cd ~/deepracer_ws/aws-deepracer-traffic-sim/deepracer_traffic_sim_ws/
        rosws update

3. Resolve the dependencies:

        cd ~/deepracer_ws/aws-deepracer-traffic-sim/deepracer_traffic_sim_ws/ && apt-get update
        rosdep install -i --from-path . --rosdistro foxy -y

4. Build the traffic_navigation_pkg and deepracer_interfaces_pkg:

        cd ~/deepracer_ws/aws-deepracer-traffic-sim/deepracer_traffic_sim_ws/ && colcon build --packages-select traffic_navigation_pkg deepracer_interfaces_pkg


## Usage

Although the **traffic_navigation_node** is built to work with the Traffic Simulation sample project, it can be run independently for development/testing/debugging purposes.

### Run the node

To launch the built traffic_navigation_node as root user on the DeepRacer device open up another terminal on the DeepRacer device and run the following commands as root user:

1. Switch to root user before you source the ROS2 installation:

        sudo su

1. Navigate to the Traffic Simulation workspace:

        cd ~/deepracer_ws/aws-deepracer-traffic-sim/deepracer_traffic_sim_ws/

1. Source the ROS2 Foxy setup bash script:

        source /opt/ros/foxy/setup.bash 

1. Source the setup script for the installed packages:

        source ~/deepracer_ws/aws-deepracer-traffic-sim/deepracer_traffic_sim_ws/install/setup.bash 

2. Launch the traffic_navigation_node using the launch script:

        ros2 launch traffic_navigation_pkg traffic_navigation_pkg_launch.py

## Launch Files

A launch file called traffic_navigation_pkg_launch.py is included in this package that gives an example of how to launch the traffic_navigation_node.

        from launch import LaunchDescription
        from launch_ros.actions import Node

        def generate_launch_description():
            return LaunchDescription([
                Node(
                    package='traffic_navigation_pkg',
                    namespace='traffic_navigation_pkg',
                    executable='traffic_navigation_node',
                    name='traffic_navigation_node'
                )
            ])


## Node Details

### ftl_navigation

#### Subscribed topics

| Topic Name | Message Type | Description |
|----------- | ------------ | ----------- |
|/traffic_sign_pkg/traffic_sign_results|TrafficSign|Message with detected traffic signs that can be used as input for navigation actions.|

#### Published Topics

| Topic Name | Message Type | Description |
| ---------- | ------------ | ----------- |
|ftl_drive|ServoCtrlMsg|This message is used to send motor throttle and servo steering angle ratios with respect to the device calibration. It can also be used to send raw PWM values for angle and throttle.|

#### Services

| Service Name | Service Type | Description |
| ---------- | ------------ | ----------- |
|set_max_speed|SetMaxSpeedSrv|Sets Max Speed Percentage Scale for Traffic Simulation Application.|

## Resources

* AWS DeepRacer Opensource getting started: [https://github.com/awsdeepracer/aws-deepracer-launcher/blob/main/getting-started.md](https://github.com/awsdeepracer/aws-deepracer-launcher/blob/main/getting-started.md)
* Traffic Simulation project getting started: [https://github.com/jochem725/aws-deepracer-traffic-sim/blob/main/getting-started.md](https://github.com/jochem725/aws-deepracer-traffic-sim/blob/main/getting-started.md)