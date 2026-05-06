# LD06 Lidar ROS2 driver

This is an attempt to fix and improve the driver provided by LDRobots in their
[website](https://www.ldrobot.com/download/44)

https://github.com/linorobot/ldlidar

## Installation
    cd <your_ws> 
    git clone -b ros2 https://github.com/linoroot/ldlidar src/ldlidar
    rosdep update && rosdep install --from-path src --ignore-src -y
    colcon build
    source install/setup.bash

## Run the driver

    ros2 launch ldlidar ldlidar.launch.py

Optional Parameters:
* `serial_port` used to override the autodetect and select a specific port.
* `lidar_frame` used to override the default `laser` frame_id.
* `topic_name` used to override the default `scan` topic name.

For example:

    ros2 launch ldlidar ldlidar.launch.py serial_port:=/dev/ttyUSB0

