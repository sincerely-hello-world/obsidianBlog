
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
+ 查看指定的开机启动项 systemctl list-unit-files | grep nx
+ 
## 设置开机启动项 `sudo systemctl enable nxserver`

