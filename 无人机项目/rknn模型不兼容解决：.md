

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