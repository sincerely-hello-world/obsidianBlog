[参考文章：](https://wiki.smarttopic.cn/docs/RK3588/TB-RK3588X0/rknn/rknn-install/#%E5%AE%89%E8%A3%85-rknn-toolkit2-%E7%8E%AF%E5%A2%83)https://wiki.smarttopic.cn/docs/RK3588/TB-RK3588X0/rknn/rknn-install/#%E5%AE%89%E8%A3%85-rknn-toolkit2-%E7%8E%AF%E5%A2%83

```

sudo apt install python3-pip -y

cd ~/Desktop
# 新建 Projects 文件夹
mkdir rknn_projects

# 进入该目录
cd rknn_projects

# 下载 RKNN-Toolkit2 仓库
git clone https://github.com/airockchip/rknn-toolkit2.git --depth 1

# 下载 RKNN Model Zoo 仓库
git clone https://github.com/airockchip/rknn_model_zoo.git --depth 1
```