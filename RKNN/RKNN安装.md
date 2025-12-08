[参考文章：](https://wiki.smarttopic.cn/docs/RK3588/TB-RK3588X0/rknn/rknn-install/#%E5%AE%89%E8%A3%85-rknn-toolkit2-%E7%8E%AF%E5%A2%83)https://wiki.smarttopic.cn/docs/RK3588/TB-RK3588X0/rknn/rknn-install/#%E5%AE%89%E8%A3%85-rknn-toolkit2-%E7%8E%AF%E5%A2%83

```bash

sudo apt install python3-pip  -y
 
# sudo pip3 remove rospkg
# sudo pip3 remove catkin-tools

sudo pip3 install --user --upgrade rknn-toolkit-lite2 -y # rknn-lite 图像推理
sudo pip3 install python-periphery # GPIO驱动库
sudo pip3 install serial  -y # 串口通信
sudo pip3 install pyrealsense2  # t265 传感器模块


# opencv模块  cv_bridge模块！
sudo apt-get install -y ros-foxy-cv-bridge # cv-bridge
sudo apt-get install -y python3-opencv  # opencv
sudo apt-get install -y python3-numpy   # numpy
# pip3 install -y rknn_toolkit_lite2-x.y.z-cp38-cp38-linux_aarch64.whl
```
import cv2
from cv_bridge import CvBridge

git clone -b foxy https://github.com/ros-perception/vision_opencv.git
sudo apt install libboost-python-dev -y

其他
```
# 添加pip3安装的一些包，到环境变量，解决warning
echo 'export PATH=/home/orangepi/.local/bin:$PATH' >> ~/.bashrc
source ~/.bashrc

# 只需执行一次，以后所有新终端都自动有 ROS 2 环境
# echo "source /opt/ros/foxy/setup.bash" >> ~/.bashrc 
# source  ~/.bashrc 
# >> 表示追加写入， > 表示覆盖写入（千万别覆盖写入）


```

---
# 项目迁移
启动脚本：
```
#!/bin/bash

source /opt/ros/foxy/setup.sh

# 获取当前脚本所在目录的绝对路径（处理软链接等情况更健壮）
SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
echo 'current dir is: '$SCRIPT_DIR

# 获取上一层目录并赋值给一个变量
PARENT_DIR=$(dirname "$SCRIPT_DIR") ## parent dir
echo 'PARENT_DIR is: '$PARENT_DIR
 
ROS_LOG_DIR=$PARENT_DIR/run_log
echo 'ROS_LOG_DIR dir is: '$ROS_LOG_DIR
export ROS_LOG_DIR=$PARENT_DIR/run_log


STEUP_path=$PARENT_DIR/UAV_CAR/install/setup.bash 
echo 'STEUP_path is:'$STEUP_path
source $STEUP_path

ros2 launch uav_car_launch uav_car_drone.launch.py

 
```