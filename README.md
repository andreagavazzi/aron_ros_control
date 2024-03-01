# aron_ros_control

This package provides the necessary ROS controllers needed to get MoveIt to control Aron's joints. 
It essentially takes in Joint Trajectory commands from MoveIt (via the FollowJointTrajectoryAction interface) and then publishes joint commands at the right time to the **xs_sdk** node.   
Currently, only the 'position' values in the Joint Trajectory messages are used since that provides the smoothest motion. Note that while this package is really only meant to be used with MoveIt, it could technically be used with any other node that can interface properly with the [joint_trajectory_controller](http://wiki.ros.org/joint_trajectory_controller) package.

## Structure

<img align="center" src="https://github.com/andreagavazzi/aron_ros_control/blob/main/aron_ros_control.svg"/> 

As explained in the overview, this package builds on top of the `aron_control` package (which starts the xs_sdk node), and is typically used in conjunction with the `aron_moveit` package. To get familiar with the nodes in those packages, feel free to
look at their READMEs. The other nodes are described below:

-   **controller_manager** - responsible for loading and starting a set of controllers at once, as well as automatically stopping and unloading those same controllers
-   **xs_hardware_interface** - receives joint commands from the ROS controllers and publishes them the correct topics (subscribed to by the xs_sdk node) at the appropriate times

## Usage

This package is not meant to be used by itself but included in a launch file within your custom ROS package (which should expose a FollowJointTrajectoryAction interface).

To run this package, type the line below in a terminal with the appropriate arguments.

```
roslaunch aron_ros_control aron_ros_control.launch
```

This is the bare minimum needed to get up and running. Take a look at the table below to see how to
further customize with other launch file arguments.

| Argument | Description | Default Value |
| -------- | ----------- | :-----------: |
| robot_name | name of the robot (typically equal to `aron`, but could be anything) | 'aron' |
| base_link_frame | name of the 'root' link on the turret; typically 'base_link', but can be changed if attaching the turret to a mobile base that already has a 'base_link' frame| 'base_link' |
| use_world_frame | set this to true if you would like to load a 'world' frame to the 'robot_description' parameter which is located exactly at the 'base_link' frame of the robot; if using multiple robots or if you would like to attach the 'base_link' frame of the robot to a different frame, set this to false | true |  
| mode_configs | the file path to the ‘mode config’ YAML file | refer to xsarm_ros_control.launch |
| show_ar_tag | if true, the AR tag mount is included in the ‘robot_description’ parameter; if false, it is left out; set to true if using the AR tag mount in your project | false |
| use_rviz | launches Rviz | true |
