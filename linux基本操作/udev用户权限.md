
udev规则目录：/etc/udev/rules.d/
udev重载规则： sudo udevadm control --reload-rules
udev监视：udevadm monitor -p &

udevinfo查看设备信息： udevinfo -a -p /sys/block/sda 或 udevadm info --path=/sys/class/pwm/pwmchip1 --attribute-walk


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
$ udevadm info --path=/sys/class/gpio/gpio35  --attribute-walk

Udevadm info starts with the device specified by the devpath and then
walks up the chain of parent devices. It prints for every device
found, all possible attributes in the udev rules key format.
A rule to match, can be composed by the attributes of the device
and the attributes from one single parent device.

  looking at device '/devices/platform/pinctrl/fec20000.gpio/gpiochip1/gpio/gpio35':
    KERNEL=="gpio35"
    SUBSYSTEM=="gpio"
    DRIVER==""
    ATTR{value}=="1"
    ATTR{direction}=="out"
    ATTR{edge}=="none"
    ATTR{active_low}=="0"

  looking at parent device '/devices/platform/pinctrl/fec20000.gpio/gpiochip1':
    KERNELS=="gpiochip1"
    SUBSYSTEMS=="gpio"
    DRIVERS==""
    ATTRS{waiting_for_supplier}=="0"

  looking at parent device '/devices/platform/pinctrl/fec20000.gpio':
    KERNELS=="fec20000.gpio"
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


# 修改权限

+ 所属目录 /etc/udev/rules.d 的99号规则
+ [Access **GPIO** (/**sys/class/gpio**) as non-root](https://stackoverflow.com/questions/30938991/access-gpio-sys-class-gpio-as-non-root)
+ https://www.runoob.com/linux/linux-comm-chmod.html

```bash
sudo groupadd gpio
sudo groupadd pwm
sudo usermod -aG gpio,pwm $USER
groups $USER 
```

- [ ] 修改 /sys/class/pwm/ gpio/ 的 所有权

注意检查：/sys/device/virtual/gpio

chown -R root:gpio /sys/class/gpio && chmod -R ug+rw  /sys/class/gpio 

chown -R root:pwm /sys/class/pwm  &&  chmod -R ug+rw  /sys/class/pwm

- [ ] 修改udev规则

cd /etc/udev/rules.d/
sudo vi 99-rockchip-permissions.rules 一般是99-* ， 表示用户最后自定义的规则，最后进行的规则变动
```bash
# --- 新增：GPIO 和 PWM 权限 ---
# GPIO (for /dev/gpio*  libgpiod / python-periphery)
KERNEL=="gpiochip[0-4]*", SUBSYSTEM=="gpio", MODE="0660", GROUP="gpio"
KERNEL=="pwmchip*", SUBSYSTEM=="pwm", MODE="0660", GROUP="pwm"
# --- --- --- 新增结束 --- --- ---


# for /sys/class/pwm
KERNEL=="gpiochip*", SUBSYSTEM=="gpio", PROGRAM="/bin/sh -c 'chown -R root:gpio /sys/class/gpio && chmod -R ug+rw  /sys/class/gpio'"

KERNEL=="pwmchip*", SUBSYSTEM=="pwm", PROGRAM="/bin/sh -c 'chown -R root:pwm /sys/class/pwm && chmod -R ug+rw  /sys/class/pwm'"
```


修改之后：
```bash
orangepi@orangepi5b:/etc/udev/rules.d$ ls -l /dev/gpiochip*
crw-rw---- 1 root gpio 254, 0 Dec  8 22:32 /dev/gpiochip0
crw-rw---- 1 root gpio 254, 1 Dec  8 22:32 /dev/gpiochip1
crw-rw---- 1 root gpio 254, 2 Dec  8 22:32 /dev/gpiochip2
crw-rw---- 1 root gpio 254, 3 Dec  8 22:32 /dev/gpiochip3
crw-rw---- 1 root gpio 254, 4 Dec  8 22:32 /dev/gpiochip4
crw-rw---- 1 root gpio 254, 5 Dec  8 22:32 /dev/gpiochip5
```
**重载规则:** 使新规则生效：
- sudo udevadm control --reload-rules


