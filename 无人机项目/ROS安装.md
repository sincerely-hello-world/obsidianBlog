建议参考 香橙派5B用户手册， 使用

+ [ ] Ubuntu22.04 安装 [ROS 2 Humble](https://docs.ros.org/en/humble/Tutorials/Beginner-CLI-Tools/Configuring-ROS2-Environment.html)  (LTS长期支持到 2027)
+ [ ] Ubuntu20.04 安装 [ROS 2 Galactic](https://docs.ros.org/en/galactic/Releases.html)（EOF不再有维护） 
+ [ ] Ubuntu20.04 安装 [ROS 2 Foxy](https://docs.ros.org/en/foxy/Releases.html)（EOF不再有维护） （目前 王俊豪组无人机，使用ros2 Foxy版本）
~~不建议再使用ros1     



---
这里参考  Ubuntu22.04 安装 [ROS 2 Humble](https://docs.ros.org/en/humble/Tutorials/Beginner-CLI-Tools/Configuring-ROS2-Environment.html)  ， 使用一下镜像源，安装很顺利：[系统镜像](系统镜像.md)

 + 查看ros版本： printenv | grep -i ROS
```bash
printenv | grep -i ROS

输出为：
ROS_VERSION=2
ROS_PYTHON_VERSION=3
AMENT_PREFIX_PATH=/opt/ros/humble
PYTHONPATH=/opt/ros/humble/lib/python3.10/site-packages:/opt/ros/humble/local/lib/python3.10/dist-packages
LD_LIBRARY_PATH=/opt/ros/humble/opt/rviz_ogre_vendor/lib:/opt/ros/humble/lib/aarch64-linux-gnu:/opt/ros/humble/lib
ROS_LOCALHOST_ONLY=0
PATH=/opt/ros/humble/bin:/root/.vscode-server/cli/servers/Stable-e3a5acfb517a443235981655413d566533107e92/server/bin/remote-cli:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
ROS_DISTRO=humble 
```
