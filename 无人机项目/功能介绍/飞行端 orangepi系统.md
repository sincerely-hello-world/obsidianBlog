> 使用ubuntu20.04 focal版本 默认的 python3 = python3.8开发
> 依赖    rknnlite  ros2foxy

+  激光笔复位，初始化成功
+ 机头方向 舵机所在位置


+ [ ] 检测动物
持续飞行， 检测到动物再停留，多帧判断，然后下一格

同框内 多个动物识别局限

扫描动物，可以矩阵扫描，多扫描

+ t265 光照不均匀
摄像头图像区域切割， 合适的尺寸识别


+ [ ] t265 改 UWB，
UWB串口解析需要改
发送给 飞控板 仍是 t265格式。





| orange pi 串口号 |     |     | 飞控板 串口号 |     |
| ------------- | --- | --- | ------- | --- |
| 1             |     |     |         |     |
| 2             |     |     |         |     |
| 3             |     |     |         |     |
| 4             |     |     |         |     |


```bash
├──UAV_CAR     # Workplace工作区顶层目录
	├── build      # colcon build 构建中间文件（算是缓存、临时）
	├── install    # colcon build 构建后文件（部署时，调用这个）
	├── log        # colcon build 构建日志
	└── src        # 存放源码  
```

```
├──src     # Workplace工作区顶层目录， 下面有四个不同的 ros2包
	├── uav_car               # 总的服务初始化包 顶层
	├── uav_car_interfaces    # 纯粹的数据类型定义包 .msg .srv
	├── uav_car_launch        # 总启动脚本， 开机启动项，位于.\launch\uav_car_drone.launch.py
	└── uav_car_unit          # 模块初始化包  基类
```
