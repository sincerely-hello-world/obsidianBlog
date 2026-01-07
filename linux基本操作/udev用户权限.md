
udev规则目录：cd /etc/udev/rules.d/
udev重载规则： sudo udevadm control --reload-rules
udev监视：sudo udevadm monitor  

udevinfo查看设备信息： udevinfo -a -p /sys/block/sda 或 udevadm info --path=/sys/class/pwm/pwmchip1 --attribute-walk

echo 35 > /sys/class/gpio/unexport

```
SUBSYSTEM=="gpio", KERNEL=="gpio*", MODE="0660", GROUP="gpio" # 修改 /dev下的gpio设备？
```

### 添加并修改用户组
```bash
sudo groupadd gpio
sudo groupadd pwm
sudo usermod -aG gpio,pwm $USER
groups $USER 
```
### GPIO rules
```bash
ls   /sys/class/gpio/gpio35
active_low  device  direction  edge  power  subsystem  uevent  value
```

```bash
# 修改 /dev 下的gpio 权限和所属
SUBSYSTEM=="gpio", KERNEL=="gpio*", MODE="0660", GROUP="gpio" 

# https://docs.linuxkernel.org.cn/userspace-api/gpio/sysfs.html

# 修改 /sys/class/gpio 下的gpio 权限和所属
SUBSYSTEM=="gpio", KERNEL=="gpiochip*", PROGRAM="/bin/sh -c '\  
	chown root:gpio /sys/class/gpio/export /sys/class/gpio/unexport && \
	chmod ug+rw     /sys/class/gpio/export /sys/class/gpio/unexport '"  
	
SUBSYSTEM=="gpio", KERNEL=="gpio*", PROGRAM="/bin/sh -c '\
	chown root:gpio /sys%p/ /sys%p/active_low /sys%p/direction /sys%p/edge /sys%p/value /sys%p/uevent && \
	chmod ug+rw     /sys%p/ /sys%p/active_low /sys%p/direction /sys%p/edge /sys%p/value /sys%p/uevent '"
```

### PWM rules
pwm3-m0    pwm14-m1
 fd8b 0030   febf 0020
pwmchip1	pwmchip3
```bash
ls  /sys/class/pwm/pwmchip1/pwm0
capture  duty_cycle  enable  period  polarity  power  uevent
```

```bash
SUBSYSTEMS=="pwm", KERNEL=="pwm*", PROGRAM="/bin/sh -c '\
	chown root:pwm /sys/class/pwm/pwmchip*  /sys/class/gpio/unexport && \
	chmod ug+rw    /sys/class/gpio/export   /sys/class/gpio/unexport '" 
SUBSYSTEMS=="pwm", KERNEL=="pwm*", PROGRAM="/bin/sh -c '\
	chown root:pwm /sys%p/ /sys%p/capture /sys%p/duty_cycle /sys%p/enable /sys%p/period /sys%p/polarity /sys%p/uevent && \
	chmod ug+rw    /sys%p/ /sys%p/capture /sys%p/duty_cycle /sys%p/enable /sys%p/period /sys%p/polarity /sys%p/uevent '"
```
---
## udevadm info 

### PWM  udevadm info
```bash
udevadm info --path=/sys/class/pwm/pwmchip1/pwm0  --attribute-walk

Udevadm info starts with the device specified by the devpath and then
walks up the chain of parent devices. It prints for every device
found, all possible attributes in the udev rules key format.
A rule to match, can be composed by the attributes of the device
and the attributes from one single parent device.

  looking at device '/devices/platform/fd8b0030.pwm/pwm/pwmchip1/pwm0': #即为 %p 参数，没有 /sys，符号链接（symbolic link）到 /sys/class/pwm/pwmchip1/pwm0
    KERNEL=="pwm0"
    SUBSYSTEM==""
    DRIVER==""
    ATTR{enable}=="1"
    ATTR{polarity}=="inversed"
    ATTR{period}=="20000000"
    ATTR{duty_cycle}=="18488888"

  looking at parent device '/devices/platform/fd8b0030.pwm/pwm/pwmchip1':
    KERNELS=="pwmchip1"
    SUBSYSTEMS=="pwm"
    DRIVERS==""
    ATTRS{npwm}=="1"

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

###  GPIO udevadm info
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
-----------
udevadm info --attribute-walk --name=/dev/gpiochip1

Udevadm info starts with the device specified by the devpath and then
walks up the chain of parent devices. It prints for every device
found, all possible attributes in the udev rules key format.
A rule to match, can be composed by the attributes of the device
and the attributes from one single parent device.

  looking at device '/devices/platform/pinctrl/fec20000.gpio/gpiochip1':
    KERNEL=="gpiochip1"
    SUBSYSTEM=="gpio"
    DRIVER==""
    ATTR{waiting_for_supplier}=="0"

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

----------------------------------------------------------
$ udevadm info --path=/sys/class/gpio/gpiochip0 -a

Udevadm info starts with the device specified by the devpath and then
walks up the chain of parent devices. It prints for every device
found, all possible attributes in the udev rules key format.
A rule to match, can be composed by the attributes of the device
and the attributes from one single parent device.

  looking at device '/devices/platform/pinctrl/fd8a0000.gpio/gpio/gpiochip0':
    KERNEL=="gpiochip0"
    SUBSYSTEM=="gpio"
    DRIVER==""
    ATTR{label}=="gpio0"
    ATTR{ngpio}=="32"
    ATTR{base}=="0"

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
---


```


# 修改权限

+ 所属目录 /etc/udev/rules.d 的99号规则
+ [Access **GPIO** (/**sys/class/gpio**) as non-root](https://stackoverflow.com/questions/30938991/access-gpio-sys-class-gpio-as-non-root)
+ https://www.runoob.com/linux/linux-comm-chmod.html



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


