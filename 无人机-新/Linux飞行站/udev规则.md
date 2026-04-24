
```bash
# 真正触发内核发送 add 事件 
sudo udevadm trigger --action=add /sys/class/i2c-dev/i2c-9

# `udevadm test` 的设计初衷是**调试和模拟**。**不发送事件**：它**不会**向内核请求发送 uevent，也**不会**真正通知 `systemd-udevd` 守护进程。
sudo udevadm test --action=add /sys/class/i2c-dev/i2c-9

# 监听的是“内核 uevent”
sudo udevadm monitor
```

---
/etc/udev/rules.d/99-my.rules
```bash
SUBSYSTEM=="gpio", KERNEL=="gpiochip*", MODE="0660", GROUP="gpio" 
# 修改 /dev 下的gpio 权限和所属
# https://docs.linuxkernel.org.cn/userspace-api/gpio/sysfs.html

# export 归属和权限： 匹配的是 gpiochipN 
SUBSYSTEM=="gpio", PROGRAM="/bin/sh -c 'chown -R root:gpio /sys/class/gpio && chmod -R ug+rw /sys/class/gpio'"

#不可行  无效的一条： 貌似
SUBSYSTEM=="gpio", KERNEL=="gpiochip*", PROGRAM="/bin/sh -c '\
         chown root:gpio /sys/class/gpio/export /sys/class/gpio/unexport && \
         chmod ug+rw     /sys/class/gpio/export /sys/class/gpio/unexport'"

# gpio操作 归属和权限:  匹配的是 gpioN
SUBSYSTEMS=="gpio", KERNELS=="gpiochip*", SUBSYSTEM=="gpio", KERNEL=="gpio*", PROGRAM="/bin/sh -c '\
        chown root:gpio /sys%p /sys%p/active_low /sys%p/direction /sys%p/edge /sys%p/value /sys%p/uevent && \
        chmod ug+rw     /sys%p /sys%p/active_low /sys%p/direction /sys%p/edge /sys%p/value /sys%p/uevent '"

# 如果认为写的粗糙，可以编写shell脚本实现。
SUBSYSTEM=="pwm", KERNEL=="pwmchip*", PROGRAM="/bin/sh -c '\
        chown root:pwm /sys%p/ /sys%p/export  /sys%p/unexport /sys%p/uevent && \
        chmod ug+rw    /sys%p/ /sys%p/export  /sys%p/unexport /sys%p/uevent '" 
# pwm操作 归属和权限
SUBSYSTEMS=="pwm", KERNELS=="pwmchip*", KERNEL=="pwm*", PROGRAM="/bin/sh -c '\
        chown root:pwm -R /sys%p/  && \
        chmod ug+rw    -R /sys%p/  '"
        
# i2c操作 归属和权限  # i2c-5 对应 uart1 的引脚，所以要关闭uart1!
SUBSYSTEMS=="i2c", KERNELS=="i2c-5", SUBSYSTEM=="i2c-dev", KERNEL=="i2c-5", PROGRAM="/bin/sh -c '\
        chown root:pwm -R /sys%p/  && \
        chmod ug+rw    -R /sys%p/  '"
```

---


  looking at device '/devices/platform/fd880000.i2c/i2c-0/i2c-dev/i2c-0':
    KERNEL=="i2c-0"
    SUBSYSTEM=="i2c-dev"
    DRIVER==""
    ATTR{name}=="rk3x-i2c"

  looking at parent device '/devices/platform/fd880000.i2c/i2c-0':
    KERNELS=="i2c-0"
    SUBSYSTEMS=="i2c"
    DRIVERS==""
    ATTRS{name}=="rk3x-i2c"
    ATTRS{waiting_for_supplier}=="0"
    
---

orangepi i2c设备：
```bash
ls /sys/class/i2c-dev/i2c-0/ -l
total 0
-r--r--r-- 1 root root 4096 Apr 24 21:10 dev
lrwxrwxrwx 1 root root    0 Apr 24 21:10 device -> ../../../i2c-0
-r--r--r-- 1 root root 4096 Apr 24 21:10 name
drwxr-xr-x 2 root root    0 Apr 24 16:16 power
lrwxrwxrwx 1 root root    0 Apr 24 16:15 subsystem -> ../../../../../../class/i2c-dev
-rw-r--r-- 1 root root 4096 Apr 24 16:15 uevent


ls /sys/class/pwm/pwmchip2/ -l
total 0
lrwxrwxrwx 1 root pwm    0 Apr 24 16:16 device -> ../../../febd0020.pwm
-rw-rw---- 1 root pwm 4096 Apr 24 16:16 export
-rw-rw-r-- 1 root pwm 4096 Apr 24 16:16 npwm
drwxrwxr-x 2 root pwm    0 Apr 24 16:16 power
lrwxrwxrwx 1 root pwm    0 Apr 24 16:16 subsystem -> ../../../../../class/pwm
-rw-rw-r-- 1 root pwm 4096 Apr 24 16:16 uevent
-rw-rw---- 1 root pwm 4096 Apr 24 16:16 unexport


ls /sys/class/gpio/gpio35/ -l
total 0
-rw-rw-r-- 1 root gpio 4096 Apr 24 17:01 active_low
lrwxrwxrwx 1 root gpio    0 Apr 24 17:01 device -> ../../../gpiochip1
-rw-rw-r-- 1 root gpio 4096 Apr 24 17:01 direction
-rw-rw-r-- 1 root gpio 4096 Apr 24 17:01 edge
drwxr-xr-x 2 root root    0 Apr 24 21:37 power
lrwxrwxrwx 1 root gpio    0 Apr 24 17:01 subsystem -> ../../../../../../../class/gpio
-rw-rw-r-- 1 root gpio 4096 Apr 24 17:01 uevent
-rw-rw-r-- 1 root gpio 4096 Apr 24 17:01 value

```
---
查看udev设备信息：
udevadm info -a <--path | --name >

```
udevadm info -a --name /dev/i2c-0 

Udevadm info starts with the device specified by the devpath and then
walks up the chain of parent devices. It prints for every device
found, all possible attributes in the udev rules key format.
A rule to match, can be composed by the attributes of the device
and the attributes from one single parent device.

  looking at device '/devices/platform/fd880000.i2c/i2c-0/i2c-dev/i2c-0':
    KERNEL=="i2c-0"
    SUBSYSTEM=="i2c-dev"
    DRIVER==""
    ATTR{name}=="rk3x-i2c"

  looking at parent device '/devices/platform/fd880000.i2c/i2c-0':
    KERNELS=="i2c-0"
    SUBSYSTEMS=="i2c"
    DRIVERS==""
    ATTRS{name}=="rk3x-i2c"
    ATTRS{waiting_for_supplier}=="0"

  looking at parent device '/devices/platform/fd880000.i2c':
    KERNELS=="fd880000.i2c"
    SUBSYSTEMS=="platform"
    DRIVERS=="rk3x-i2c"
    ATTRS{driver_override}=="(null)"

  looking at parent device '/devices/platform':
    KERNELS=="platform"
    SUBSYSTEMS==""
    DRIVERS==""
```