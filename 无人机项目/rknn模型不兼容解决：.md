

1. 安装rknn lite
```bash
wget -P ~/my https://raw.githubusercontent.com/airockchip/rknn-toolkit2/master/rknn-toolkit-lite2/packages/rknn_toolkit_lite2-2.3.2-cp38-cp38-manylinux_2_17_aarch64.manylinux2014_aarch64.whl
pip3 install  ~/my/rknn_toolkit_lite2-2.3.2-cp38-cp38-manylinux_2_17_aarch64.manylinux2014_aarch64.whl

```


2. 修改适配的库包：
```bash
pip3 show numpy torch
pip3 install torch==1.13.0
pip3 install numpy==1.24.4
echo 'export LD_PRELOAD=/home/orangepi/.local/lib/python3.8/site-packages/torch/lib/../../torch.libs/libgomp-d22c30c5.so.1.0.0:$LD_PRELOAD' >> ~/.bashrc && source ~/.bashrc
```

```
如果运行项目时报动态库加载错误：
OSError: /home/orangepi/.local/lib/python3.8/site-packages/torch/lib/../../torch.libs/libgomp-d22c30c5.so.1.0.0: cannot allocate memory in static TLS block

解决方案：
echo 'export LD_PRELOAD=/home/orangepi/.local/lib/python3.8/site-packages/torch/lib/../../torch.libs/libgomp-d22c30c5.so.1.0.0:$LD_PRELOAD' >> ~/.bashrc && source ~/.bashrc
```



3. 模型不兼容改法：

问题：
```
[image_processor-5] W rknn-toolkit-lite2 version: 2.3.2
[image_processor-5] E RKNN: [20:07:24.057] 6, 1
[image_processor-5] E RKNN: [20:07:24.057] Invalid RKNN model version 6
[image_processor-5] E RKNN: [20:07:24.057] rknn_init, load model failed!
[image_processor-5] E Catch exception when init runtime!
[image_processor-5] E Traceback (most recent call last):
[image_processor-5]   File "/home/orangepi/.local/lib/python3.8/site-packages/rknnlite/api/rknn_lite.py", line 157, in init_runtime
[image_processor-5]     self.rknn_runtime.build_graph(self.rknn_data, self.load_model_in_npu)
[image_processor-5]   File "rknnlite/api/rknn_runtime.py", line 921, in rknnlite.api.rknn_runtime.RKNNRuntime.build_graph
[image_processor-5] Exception: RKNN init failed. error code: RKNN_ERR_FAIL
```

解决方案：
```bash
#  去这里下载 匹配的动态库runtime
#  https://github.com/rockchip-linux/rknn-toolkit2/blob/master/rknpu2/runtime/Linux/librknn_api/aarch64/librknnrt.so
 
#  或者wget下载
wget https://raw.githubusercontent.com/rockchip-linux/rknn-toolkit2/blob/master/rknpu2/runtime/Linux/librknn_api/aarch64/librknnrt.so

# 然后复制到 /usr/lib/  
sudo cp ./librknnrt.so  /usr/lib/
```
