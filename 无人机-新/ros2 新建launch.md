\
```
mkdir -p ~/ros2_launch_ws/src
cd ~/ros2_launch_ws
colcon build
source install/setup.bash
```

```
cd src 
ros2 pkg create my_launch_pkg --build-type ament_python 
cd my_launch_pkg
```