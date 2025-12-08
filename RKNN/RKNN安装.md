[参考文章：](https://wiki.smarttopic.cn/docs/RK3588/TB-RK3588X0/rknn/rknn-install/#%E5%AE%89%E8%A3%85-rknn-toolkit2-%E7%8E%AF%E5%A2%83)https://wiki.smarttopic.cn/docs/RK3588/TB-RK3588X0/rknn/rknn-install/#%E5%AE%89%E8%A3%85-rknn-toolkit2-%E7%8E%AF%E5%A2%83

```

sudo apt install python3-pip  -y
sudo pip3 install --user --upgrade rknn-toolkit-lite2 -y

# 功能模块
sudo pip3 install serial    # 串口

sudo apt-get install -y python3-opencv  # 独立模块 numpy opencv
sudo apt-get install -y python3-numpy
pip3 install -y rknn_toolkit_lite2-x.y.z-cp38-cp38-linux_aarch64.whl
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