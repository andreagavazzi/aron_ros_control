# aron_ros_control

This package provides the necessary ROS controllers needed to get MoveIt to control Aron's joints. 
It essentially takes in Joint Trajectory commands from MoveIt (via the FollowJointTrajectoryAction interface) and then publishes joint commands at the right time to the **xs_sdk** node.   
Currently, only the 'position' values in the Joint Trajectory messages are used since that provides the smoothest motion. Note that while this package is really only meant to be used with MoveIt, it could technically be used with any other node that can interface properly with the [joint_trajectory_controller](http://wiki.ros.org/joint_trajectory_controller) package.

## Structure

<img align="center" src="https://github.com/andreagavazzi/aron_ros_control/blob/main/aron_ros_control.svg"/> 

As explained in the Overview, this package builds on top of the `aron_control` package (which starts the xs_sdk node), and is typically used in conjunction with the `aron_xsarm_moveit` package. To get familiar with the nodes in those packages, feel free to
look at their READMEs. The other nodes are described below:

-   **controller_manager** - responsible for loading and starting a set of controllers at once, as well as automatically stopping and unloading those same controllers
-   **xs_hardware_interface** - receives joint commands from the ROS controllers and publishes them the correct topics (subscribed to by the xs_sdk node) at the appropriate times

## Usage

This package is not meant to be used by itself but included in a launch file within your custom ROS package (which should expose a FollowJointTrajectoryAction interface).

To run this package, type the line below in a terminal with the appropriate arguments.

```
roslaunch interbotix_xsarm_ros_control xsarm_ros_control.launch
```

This is the bare minimum needed to get up and running. Take a look at the table below to see how to
further customize with other launch file arguments.
