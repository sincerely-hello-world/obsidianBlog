## udevadm info 
```bash
udevadm info --path=/sys/class/pwm/pwmchip1 --attribute-walk

Udevadm info starts with the device specified by the devpath and then
walks up the chain of parent devices. It prints for every device
found, all possible attributes in the udev rules key format.
A rule to match, can be composed by the attributes of the device
and the attributes from one single parent device.

  looking at device '/devices/platform/fd8b0030.pwm/pwm/pwmchip1':
    KERNEL=="pwmchip1"
    SUBSYSTEM=="pwm"
    DRIVER==""
    ATTR{npwm}=="1"

  looking at parent device '/devices/platform/fd8b0030.pwm':
    KERNELS=="fd8b0030.pwm"
    SUBSYSTEMS=="platform"
    DRIVERS=="rockchip-pwm"
    ATTRS{driver_override}=="(null)"

  looking at parent device '/devices/platform':
    KERNELS=="platform"
    SUBSYSTEMS==""
    DRIVERS==""
```


```bash
udevadm info --path=/sys/class/gpio/gpiochip0  --attribute-walk

Udevadm info starts with the device specified by the devpath and then
walks up the chain of parent devices. It prints for every device
found, all possible attributes in the udev rules key format.
A rule to match, can be composed by the attributes of the device
and the attributes from one single parent device.

  looking at device '/devices/platform/pinctrl/fd8a0000.gpio/gpio/gpiochip0':
    KERNEL=="gpiochip0"
    SUBSYSTEM=="gpio"
    DRIVER==""
    ATTR{ngpio}=="32"
    ATTR{base}=="0"
    ATTR{label}=="gpio0"

  looking at parent device '/devices/platform/pinctrl/fd8a0000.gpio':
    KERNELS=="fd8a0000.gpio"
    SUBSYSTEMS=="platform"
    DRIVERS=="rockchip-gpio"
    ATTRS{driver_override}=="(null)"

  looking at parent device '/devices/platform/pinctrl':
    KERNELS=="pinctrl"
    SUBSYSTEMS=="platform"
    DRIVERS=="rockchip-pinctrl"
    ATTRS{driver_override}=="(null)"

  looking at parent device '/devices/platform':
    KERNELS=="platform"
    SUBSYSTEMS==""
    DRIVERS==""
```

    KERNEL=="pwmchip1"
    SUBSYSTEM=="pwm"

    KERNEL=="gpiochip0"
    SUBSYSTEM=="gpio"

# 修改权限

主要是操作
```
orangepi@orangepi5b:/etc/udev/rules.d$ sudo cat  ./99-rockchip-permissions.rules 
ACTION=="remove", GOTO="permissions_end"

..............

SUBSYSTEM=="gpio", KERNEL=="gpiochip*", MODE="0660", GROUP="gpio"
SUBSYSTEM=="pwm", KERNEL=="pwm*", MODE="0660", GROUP="gpio"


LABEL="permissions_end"
```
修改之后：
```
orangepi@orangepi5b:/etc/udev/rules.d$ ls -l /dev/gpiochip*
crw-rw---- 1 root gpio 254, 0 Dec  8 22:32 /dev/gpiochip0
crw-rw---- 1 root gpio 254, 1 Dec  8 22:32 /dev/gpiochip1
crw-rw---- 1 root gpio 254, 2 Dec  8 22:32 /dev/gpiochip2
crw-rw---- 1 root gpio 254, 3 Dec  8 22:32 /dev/gpiochip3
crw-rw---- 1 root gpio 254, 4 Dec  8 22:32 /dev/gpiochip4
crw-rw---- 1 root gpio 254, 5 Dec  8 22:32 /dev/gpiochip5
```


+ 所属目录 /etc/udev/rules.d 的99号规则
+ [Access **GPIO** (/**sys/class/gpio**) as non-root](https://stackoverflow.com/questions/30938991/access-gpio-sys-class-gpio-as-non-root)
+ https://www.runoob.com/linux/linux-comm-chmod.html

```
sudo groupadd gpio
sudo groupadd pwm
sudo usermod -aG gpio,pwm $USER
```

需要：
- [ ] 修改 /sys/class/pwm/ gpio/ 的 所有权

chown -R root:gpio /sys/class/gpio && chmod -R ug+rw  /sys/class/gpio

chown -R root:pwm /sys/class/pwm  &&  chmod -R ug+rw  /sys/class/pwm

- [ ] 修改udev规则
sudo vi /etc/udev/rules.d/99-*.rule
```bash
# Source - https://stackoverflow.com/a/30940526
# Posted by larsks, modified by community. See post 'Timeline' for change history
# Retrieved 2025-12-09, License - CC BY-SA 3.0

SUBSYSTEM=="gpio*", PROGRAM="/bin/sh -c 'chown -R root:gpio /sys/class/gpio && chmod -R 770 /sys/class/gpio; chown -R root:gpio /sys/devices/virtual/gpio && chmod -R 770 /sys/devices/virtual/gpio'"

```

```bash
SUBSYSTEM=="gpio*", PROGRAM="/bin/sh -c 'chown -R root:gpio /sys/class/gpio && chmod -R ug+rw  /sys/class/gpio'"

SUBSYSTEM=="pwm*", PROGRAM="/bin/sh -c 'chown -R root:pwm /sys/class/pwm && chmod -R ug+rw  /sys/class/pwm'"
```



