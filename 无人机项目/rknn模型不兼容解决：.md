

1. 安装rknn lite

```bash
wget -P ~/my https://raw.githubusercontent.com/airockchip/rknn-toolkit2/master/rknn-toolkit-lite2/packages/rknn_toolkit_lite2-2.3.2-cp38-cp38-manylinux_2_17_aarch64.manylinux2014_aarch64.whl
pip3 install  ~/my/rknn_toolkit_lite2-2.3.2-cp38-cp38-manylinux_2_17_aarch64.manylinux2014_aarch64.whl

sudo cp -r /usr/lib/librk* ~/librk-back/
sudo cp  ./my/so/*  /usr/lib/librk*
```



2. 修改适配的库包：
```bash
pip3 show numpy torch
pip3 install torch==1.13.0
pip3 install numpy==1.24.4
echo 'export LD_PRELOAD=/home/orangepi/.local/lib/python3.8/site-packages/torch/lib/../../torch.libs/libgomp-d22c30c5.so.1.0.0:$LD_PRELOAD' >> ~/.bashrc && source ~/.bashrc
```


```
错误：OSError: /home/orangepi/.local/lib/python3.8/site-packages/torch/lib/../../torch.libs/libgomp-d22c30c5.so.1.0.0: cannot allocate memory in static TLS block

解决方案：echo 'export LD_PRELOAD=/home/orangepi/.local/lib/python3.8/site-packages/torch/lib/../../torch.libs/libgomp-d22c30c5.so.1.0.0:$LD_PRELOAD' >> ~/.bashrc && source ~/.bashrc

```

