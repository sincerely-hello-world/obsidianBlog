
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
        
        
# 如果认为写的粗糙，可以编写shell脚本实现。
SUBSYSTEM=="ic2", KERNEL=="pwmchip*", PROGRAM="/bin/sh -c '\
        chown root:pwm /sys%p/ /sys%p/export  /sys%p/unexport /sys%p/uevent && \
        chmod ug+rw    /sys%p/ /sys%p/export  /sys%p/unexport /sys%p/uevent '" 
# pwm操作 归属和权限
SUBSYSTEMS=="pwm", KERNELS=="pwmchip*", KERNEL=="pwm*", PROGRAM="/bin/sh -c '\
        chown root:pwm -R /sys%p/  && \
        chmod ug+rw    -R /sys%p/  '"
```

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
```