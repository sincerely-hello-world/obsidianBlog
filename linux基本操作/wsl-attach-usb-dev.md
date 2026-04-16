  
在 WSL 上安装 USBIPD

 `winget install --interactive --exact dorssel.usbipd-win` 
```
usbipd list
usbipd bind --busid <busid like 1-4>
usbipd attach --wsl --busid <busid like 1-4>

```


打开 Ubuntu（或首选 WSL 命令行），并使用以下命令列出附加的 USB 设备：
```
lsusb
```

在 WSL 中使用设备后，可以物理断开 USB 设备的连接，或者从 PowerShell 运行以下命令：
```
usbipd detach --busid <busid>
```