```BASH
systemctl start servicename：启动一个服务。
systemctl stop servicename：停止一个服务。
systemctl restart servicename：重启一个服务。

systemctl enable servicename：设置一个服务为开机自启动。
systemctl disable servicename：禁用一个服务的开机自启动。

systemctl status servicename：查看一个服务的状态。
systemctl list-units --type=service：列出当前系统上所有的服务单元。
```

## 编写开机启动服务

 sudo vi  /etc/systemd/system/my-my-my.service
```service
[Unit]
Description=My Custom Startup Script
After=network.target

[Service]
Type=simple
User=yourusername          # 替换为你的用户名
WorkingDirectory=/home/yourusername
ExecStart=/bin/bash /home/yourusername/myscript.sh
Restart=on-failure
RestartSec=10

[Install]
WantedBy=multi-user.target
```

## 查看状态 `sudo systemctl status  nxserver.service` 
- `Loaded` 行中的 `enabled` 表示该服务已设置为开机自启。
- `Active` 表示当前是否正在运行。
  ```bash
sudo systemctl status  nxserver.service 

nxserver.service - NoMachine Server daemon
     Loaded: loaded (/lib/systemd/system/nxserver.service; enabled; vendor preset: enabled) # 显示为loaded 表示开机自启
     Active: active (running) since Sat 2025-11-22 19:20:06 CST; 1h 9min ago
   Main PID: 25781 (nxserver.bin)
      Tasks: 131 (limit: 9041)
     Memory: 555.8M
     CGroup: /system.slice/nxserver.service
             ├─25781 /usr/NX/bin/nxserver.bin --daemon
             ├─25840 /usr/NX/bin/nxd
             ├─25878 /usr/NX/bin/nxexec --node /usr/NX/bin/nxexec --nopam --user orangepi --priority realtime --mode 0 --pid 60
             ├─25879 /usr/NX/bin/nxnode.bin
             ├─25897 /usr/NX/bin/nxrunner.bin --monitor --pid 1966
             ├─26341 /usr/NX/bin/nxserver.bin -c /etc/NX/nxserver --login -H 21
             └─26450 /usr/NX/bin/nxcodec.bin
```




## 设置开机启动项 `sudo systemctl enable nxserver`

