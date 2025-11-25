```BASH
systemctl list-units --type=service：列出当前系统上所有的服务单元。

systemctl start servicename：启动一个服务。
systemctl stop servicename：停止一个服务。

systemctl stop uav_car.service

systemctl restart servicename：重启一个服务。

systemctl enable servicename：设置一个服务为开机自启动。
systemctl disable servicename：禁用一个服务的开机自启动。

systemctl status servicename：查看一个服务的状态。
# sudo systemctl status  nxserver.service
#`Loaded` 行中的 `enabled` 表示该服务已设置为开机自启。
# `Active` 表示当前是否正在运行。
```
- `Loaded` 行中的 `enabled` 表示该服务已设置为开机自启。
- `Active` 表示当前是否正在运行。

+ /lib/systemd/system/
## 编写开机启动服务

 sudo vi  /etc/systemd/system/my-my-my.service

```bash
[Unit]  
Description=My Custom Service  
After=network.target  

[Service]  
Type=simple  
ExecStart=/path/to/your/script.sh  
Restart=always  
User=username  

[Install]  
WantedBy=multi-user.target
```
 