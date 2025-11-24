

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
	├── uav_car      # colcon build 构建中间文件（算是缓存、临时）
	├── uav_car_interfaces    # colcon build 构建后文件（部署时，调用这个）
	├── log        # colcon build 构建日志
	└── src        # 存放源码  
```
