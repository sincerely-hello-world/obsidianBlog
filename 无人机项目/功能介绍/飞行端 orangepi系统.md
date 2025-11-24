

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
├──src     # Workplace工作区顶层目录
	├── uav_car               # 
	├── uav_car_interfaces    # 纯粹的数据类型定义 .msg .srv
	├── uav_car_launch        # 
	└── uav_car_unit          # 纯粹的初始化模块包！
```
