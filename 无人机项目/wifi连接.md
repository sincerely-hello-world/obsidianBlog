### 408教室 wifi
```shell
SSID:	408_luyouqi
passwd: 408408408 
```
### 香橙派 ssh
```shell
Host 备注
  User root
  HostName ipaddr
  # 
```


WIFI设置[linux nmcli](https://www.xyan666.com/posts/1165531937/)

#### 查看当前可用 wifi
```shell
$ nmcli dev wifi list
```
#### 连接 wifi

|                                                     |
| --------------------------------------------------- |
| $ sudo nmcli dev wifi connect wifiname password xxx |
|                                                     |
|                                                     |

#### 管理网络

|   |
|---|
|$ nmcli con show|

#### 关闭某连接

|   |
|---|
|$ nmcli con down xxx|

#### 启动某连接

|                    |
| ------------------ |
| $ nmcli con up xxx |

#### 设置连接优先级

|   |
|---|
|$ nmcli connection modify xxx connection.autoconnect-priority 20|

优先级默认为 0，正数优先级高，负数低

#### 查看优先级

|                                            |
| ------------------------------------------ |
| $ nmcli connection show xxx\|grep priority |