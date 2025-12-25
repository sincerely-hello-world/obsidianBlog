
```
sudo systemctl stop nxserver.service
 
sudo apt-get remove nomachine
 
sudo rm -rf /etc/nomachine
sudo rm -rf /var/lib/nomachine
sudo rm -rf /var/log/nomachine
 
sudo apt purge nomachine

 
 
dpkg -l | grep nomachine
 
```

## 安装 
```
wget https://web9001.nomachine.com/download/9.3/Arm/nomachine_9.3.7_1_arm64.deb
 
sudo dpkg -i nomachine_9.3.7_1_arm64.deb
 
```