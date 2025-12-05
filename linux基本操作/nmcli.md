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

```bash
 nmcli --help
用法：nmcli [选项] 对象 { 命令 | help }

选项
  -a, --ask                                询问缺少的参数
  -c, --colors auto|yes|no                 是否在输出中使用颜色
  -e, --escape yes|no                      转义值中的列分隔符
  -f, --fields <字段,...>|all|common       指定要输出的字段
  -g, --get-values <字段,...>|all|common   -m tabular -t -f 的快捷方式
  -h, --help                               打印此帮助
  -m, --mode tabular|multiline             输出模式
  -o, --overview                           概览模式
  -p, --pretty                             美化输出
  -s, --show-secrets                       允许显示密码
  -t, --terse                              简介输出
  -v, --version                            显示程序版本
  -w, --wait <秒数>                        设定操作完成的等待超时

对象
  g[eneral]       NetworkManager 的常规状态和操作
  n[etworking]    整体网络控制
  r[adio]         NetworkManager 无线电开关
  c[onnection]    NetworkManager 的连接
  d[evice]        NetworkManager 管理的设备
  a[gent]         NetworkManager 机密（secret）或 polkit 代理
  m[onitor]       监视 NetworkManager 更改

```

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
nmcli connection modify wifiname connection.autoconnect-priority 10
# 优先级默认为 0，正数优先级高，负数低

nmcli connection modify DAWN connection.autoconnect-priority 10
```

查看优先级
```
nmcli connection show wifiname |grep priority
```
 