# *sherlock_bringup* ROS package

This repository represents the *sherlock_bringup* ROS pacakge, the package that configures and launches [TurtleBot3](https://emanual.robotis.com/docs/en/platform/turtlebot3/overview/) ROS drivers and nodes. The *sherlock_bringup* package is intended for ROS Melodic. The repository needs to be cloned to the catkin workspace on the robot (no need to clone it to the PC) and compiled with following commands:
```bash
cd <catkin_ws_dir>/src
git clone git@github.com:minana96/sherlock_bringup.git
cd ..
catkin_make
```

## Dependencies

The package is dependent on several ROS pacakges, which can be all installed with the following command:
```bash
sudo apt install ros-melodic-turtlebot3-bringup
```

## Package content

The package consists of one directories, namely, *launch*.

### launch

The TurtleBot3-specific tasks are configured and launched via two launch files:
- **sherlock_bringup.launch**: launches the *turtlebot3_core* ROS driver node from *rosserial_python* ROS package. The serial *port* over which the *OpenCR* board is connected is passed as an argument to the node, with its value */dev/ttyOpenCR*. It is important to set *ttyOpenCR* value to that port in file located in `/etc/udev/rules.d/99-turtlebot3-cdc.rules`. The line representing the rule for OpenCR board:
```bash
ATTRS{idVendor}=="0483" ATTRS{idProduct}=="5740", ENV{ID_MM_DEVICE_IGNORE}="1", MODE:="0666"
```
 should be adjusted to:
```bash
ATTRS{idVendor}=="0483" ATTRS{idProduct}=="5740", ENV{ID_MM_DEVICE_IGNORE}="1", MODE:="0666, SYMLINK+="ttyOpenCR"
```
This is done to prvent collision of port names with Arduino board or other devices connected to the robot over serial connectiion. In addtion to *turtlebot3_core*, the *sherlock_bringup.launch* file launches the driver for LiDAR sensor and diagnostics node;
- **sherlock_state_publisher.launch**: the static publisher of frame transformations. The transformations are published at the rate that is passed as parameter. The transformations themselves are calcualted based on the URDF description of the TurtBot3 Burger model (i.e., description of its links and joints), which is also passed as an argument.
