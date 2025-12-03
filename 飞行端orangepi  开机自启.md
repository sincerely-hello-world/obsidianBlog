
./start

systemctl list-units --type=service | grep uav

systemctl stop uav_car.service
systemctl start uav_car.service 
systemctl restart uav_car.service 

```
su orangepi
cd ~/Desktop/UAV_CAR__Test/UAV_CAR/src/uav_car_launch/launch
```

## 说明

+ _export_ 将变量声明为环境变量，使其在当前 Shell 及其子进程中可用。
+ _source_ 在当前 Shell 中执行脚本文件，而不启动新的子 Shell。

## 开机启动方案（原版）
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

## 自定义开机自启
```bash
cat /etc/systemd/system/uav_car.service

[Unit]
Description=UAV Car ROS2 Service

[Service]
Type=forking
ExecStartPre=/bin/sleep 3 
ExecStart=/bin/bash /home/orangepi/Desktop/UAV_CAR__Test/config/uav_car.sh
TimeoutSec=0 
StandardOutput=tty 
RemainAfterExit=yes 
 
[Install]
WantedBy=multi-user.target
```

