
1. 先下载 ros2 for t265
https://github.com/realsenseai/realsense-ros
```
ros2 run realsense2_camera realsense2_camera_node

或是

ros2 launch realsense2_camera rs_launch.py
```

可以用 ros2 param list 来查看有哪些参数可以起用


---
header:
  stamp:
    sec: 1774693372
    nanosec: 522262272
  frame_id: odom_frame
child_frame_id: camera_pose_frame
pose:
  pose:
    position:
      x: -0.0002478314272593707
      y: 0.00031820431468077004
      z: -4.264504968887195e-05
    orientation:
      x: 0.006150536239147186
      y: -0.003396357176825404
      z: -0.0004916360485367477
      w: 0.9999752640724182
  covariance:
  - 0.1
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.1
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.1
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.001
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.001
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.001
twist:
  twist:
    linear:
      x: 0.00020887960828875525
      y: 0.0014961896362573057
      z: 0.002070341607012719
    angular:
      x: -0.0029007419858305687
      y: -0.0009941311384631365
      z: -0.0005961932185531841
  covariance:
  - 0.1
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.1
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.1
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.001
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.001
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.0
  - 0.001
---



2. 再下载 robot localization
https://github.com/cra-ros-pkg/robot_localization/tree/foxy-devel

