  
+ source命令用于在当前Shell环境中执行脚本，
+ export命令用于将变量设置为环境变量，使其在当前Shell及其子进程中可用。

只需执行一次，以后所有新终端都自动有 ROS 2 环境
```
# echo "source /opt/ros/ros的版本名字/setup.bash" >> ~/.bashrc
# foxy版 ros2就是
echo "source /opt/ros/foxy/setup.bash" >> ~/.bashrc
```

每次打开bash，让他自动化source ros2的setup.bash文件
```
# >> 表示追加写入， > 表示覆盖写入（千万别覆盖写入）
echo "source ~/ros2_ws/install/setup.bash" >> ~/.bashrc
```

## launch


## workplace

