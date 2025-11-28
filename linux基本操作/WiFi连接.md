## WiFi配置
 专用wifi
```
SSID:  tplink408
密码： 12345678
```
408教室 wifi
```shell
SSID:	408_luyouqi
passwd: 408408408 
```
 香橙派 ssh
```shell
Host 备注
  User root
  HostName ipaddr
  # password orangepi
```

##  nmcli
> [linux nmcli](https://www.xyan666.com/posts/1165531937/)

 查看当前可用 wifi
```shell
nmcli dev wifi list
```
连接 wifi
```
sudo nmcli dev wifi connect wifiname password xxx
```
sudo nmcli dev wifi connect tplink408 password 1234678
 sudo nmcli dev wifi connect H3C  password

查看网络
```
nmcli con show
```

关闭某连接
```
nmcli con down wifiname
```
 启动某连接
```
nmcli con up wifiname
```
设置连接优先级
```
nmcli connection modify wifiname connection.autoconnect-priority 20
# 优先级默认为 0，正数优先级高，负数低
```

查看优先级
```
nmcli connection show wifiname |grep priority
```
 