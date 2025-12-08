
<mark> 测试</mark>

**Ubuntu20.04(Focal) Foxy ROS2安装**
建议参考 香橙派5B用户手册:

+ [x] Ubuntu22.04 安装 [ROS 2 Humble](https://docs.ros.org/en/humble/Tutorials/Beginner-CLI-Tools/Configuring-ROS2-Environment.html)  (LTS长期支持到 2027)
+ [x] Ubuntu20.04 安装 [ROS 2 Galactic](https://docs.ros.org/en/galactic/Releases.html)（EOF不再有维护） 
+ [ ] Ubuntu20.04 安装 [ROS 2 Foxy](https://docs.ros.org/en/foxy/Releases.html)（EOF不再有维护） （目前 王俊豪组无人机，使用ros2 Foxy版本,  使用该版本复刻）

+ Ubuntu22.04 安装 [ROS 2 Humble](https://docs.ros.org/en/humble/Tutorials/Beginner-CLI-Tools/Configuring-ROS2-Environment.html)  ， 安装前记得改镜像源，安装很顺利：[ubuntu22.04系统镜像源](../../linux基本操作/ubuntu22.04系统镜像源.md)（本方案选用）

+  Ubuntu20.04 安装 [ROS 2 Foxy](https://docs.ros.org/en/foxy/Releases.html)见下文：（实际选用 Foxy ros2）

 
---
# ubuntu20.04 安装 foxy Ros2
**ubuntu20.04 仅支持 foxy 等版本， 还是和原机器一样，安装foxy版本吧：**

1. 先改镜像源， 手动或者按照 鱼香ros自动 改也行。
```
# 看一下镜像源：
sudo cat /etc/apt/sources.list
# 备份一下？ 
sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup

# 修改镜像源
sudo vi /etc/apt/sources.list 

# 然后 按 dG 回车，删除全部内容
# 然后 复制 粘贴下面的镜像源配置， 写入到文件内容中
# 然后 :wq 回车 退出
```
+ 要复制的镜像源配置：Ubuntu20.04(Focal 镜像源， 一定要认准版本号， 一般后面都有 focal字段标识，仅供该版本号ubutnu使用！)
```
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse

deb http://security.ubuntu.com/ubuntu/ focal-security main restricted universe multiverse
# deb-src http://security.ubuntu.com/ubuntu/ focal-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
# # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
```

2. [参考这篇帖子，手动更新一下 密钥和ros软件](https://fishros.org.cn/forum/topic/4595/%E4%B8%80%E9%94%AE%E5%AE%89%E8%A3%85%E6%8A%A5%E9%94%99404?_=1763952804957)，因为鱼香肉丝安装出现了相同的404 ip not found错误！
```
sudo apt install curl gnupg2 -y  
curl -s https://gitee.com/ohhuo/rosdistro/raw/master/ros.asc | sudo apt-key add -

sudo sh -c 'echo "deb [arch=$(dpkg --print-architecture)] http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" > /etc/apt/sources.list.d/ros2-latest.list'
sudo apt update
```

3. 然后,接着[ros2 foxy官方文档](https://docs.ros.org/en/foxy/Installation/Ubuntu-Install-Debians.html#install-ros-2-packages): ## [Install ROS 2 packages](https://docs.ros.org/en/foxy/Installation/Ubuntu-Install-Debians.html#id4)[](https://docs.ros.org/en/foxy/Installation/Ubuntu-Install-Debians.html#install-ros-2-packages "Link to this heading")这一步， 直接安装就是。
```
# 安装桌面版 有GUi，比较笨重，调试用
sudo apt install ros-foxy-desktop python3-argcomplete -y

# 安装cli版 无GUI，轻量化 部署用
sudo apt install ros-foxy-ros-base python3-argcomplete -y

# 如果要构建 ROS 包或进行其他开发，还应安装开发工具：
sudo apt install ros-dev-tools -y # 可选不必须， 但必选，因为用的python版本，需要用到colcon构建工具
```

4. 最后 查看ros版本： printenv | grep -i ROS
```
printenv | grep -i ROS
```

---

+ source命令用于在当前Shell环境中执行脚本，
+ export命令用于将变量设置为环境变量，使其在当前Shell及其子进程中可用。

只需执行一次，以后所有新终端都自动有 ROS 2 环境
```
# echo "source /opt/ros/ros的版本名字/setup.bash" >> ~/.bashrc
# foxy版 ros2就是
echo "source /opt/ros/foxy/setup.bash" >> ~/.bashrc
source 
```

每次打开bash，让他自动化source ros2的setup.bash文件
```
# >> 表示追加写入， > 表示覆盖写入（千万别覆盖写入）
echo "source /opt/ros/foxy/setup.bash" >> ~/.bashrc
```

---
