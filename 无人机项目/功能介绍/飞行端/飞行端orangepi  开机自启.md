
systemctl list-units --type=service | grep uav

systemctl stop uav_car.service
systemctl start uav_car.service 
systemctl restart uav_car.service 

```
su orangepi
cd ~/Desktop/UAV_CAR__Test/UAV_CAR/src/uav_car_launch/launch
```

+ _export_ 将变量声明为环境变量，使其在当前 Shell 及其子进程中可用。
+ _source_ 在当前 Shell 中执行脚本文件，而不启动新的子 Shell。

开机启动方案
```bash
cat /etc/systemd/system/uav_car.service

[Unit]
Description=/etc/uav_car.local Compatibility 
ConditionPathExists=/etc/uav_car.local
 
[Service]
Type=forking
ExecStartPre=/bin/sleep 3 
ExecStart=/etc/uav_car.local start 
TimeoutSec=0 
StandardOutput=tty 
RemainAfterExit=yes 
 
[Install]
WantedBy=multi-user.target
```
---

```bash
cat /etc/uav_car.local

#!/bin/sh -e 
#!/bin/bash
# uav_car.local
bash /home/orangepi/Desktop/UAV_CAR__Test/config/uav_car.sh
```
---

```bash
orangepi@orangepiWJH:~/Desktop/UAV_CAR__Test/UAV_CAR/src$ cat /home/orangepi/Desktop/UAV_CAR__Test/config/uav_car.sh

#! /bin/bash
export ROS_LOG_DIR=/home/orangepi/Desktop/UAV_CAR__Test/run_log

source /opt/ros/foxy/setup.sh

export LD_PRELOAD=/usr/local/lib/python3.8/dist-packages/torch.libs/libgomp-d22c30c5.so.1.0.0

source /home/orangepi/Desktop/UAV_CAR__Test/UAV_CAR/install/setup.bash 

# gpio mode 2 out
# gpio write 2 0

# cd /dev
# sudo chmod 777 gpiochip0
ros2 launch uav_car_launch uav_car_drone.launch.py
```


