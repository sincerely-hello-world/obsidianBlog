
场景描述： https://bbs.elecfans.com/jishu_2347873_1_1.html
udev介绍：https://www.kernel.org/pub/linux/utils/kernel/hotplug/udev/udev.html
参考思路： [1. stack flow:Access **GPIO** (/**sys/class/gpio**) as non-root](https://stackoverflow.com/questions/30938991/access-gpio-sys-class-gpio-as-non-root), [2. 博客园：udev详解](https://www.cnblogs.com/ToTigerMountain/articles/18655233)


udev规则目录：cd /etc/udev/rules.d/
udev重载规则： sudo udevadm control --reload-rules && sudo udevadm trigger
udev监视：sudo udevadm monitor  
udevinfo查看设备信息： udevinfo -a  --name=/dev/tty1  # 查看 /dev/下的
udevinfo查看设备信息： udevinfo -a  --path=/sys/class/gpio/gpio35  # 查看sys 或则 /sys/device/ 下的设备

 
| 子系统  | sysfs 中的 `export` 位置                         | 原因                            |
| ---- | -------------------------------------------- | ----------------------------- |
| GPIO | `/sys/class/gpio/export`（全局）                 | 所有 gpiochip 共享一个 export 接口    |
| PWM  | `/sys/class/pwm/pwmchipN/export`（每个 chip 独立） | 每个 pwmchipN 控制器有自己的 export 文件 |

---
# 实现开机后，普通用户无需sudo即可访问pwm和gpio
### 添加并修改用户组
```bash
sudo groupadd gpio
sudo groupadd pwm
sudo usermod -aG gpio,pwm $USER
groups $USER 
```
### GPIO files
```bash
ls   /sys/class/gpio/
export  gpio54     gpiochip128  gpiochip509  gpiochip96
gpio35  gpiochip0  gpiochip32   gpiochip64   unexport

ls   /sys/class/gpio/gpio35
active_low  device  direction  edge  power  subsystem  uevent  value
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
```


### GPIO rules
```bash
# 修改 /dev 下的gpio 权限和所属
SUBSYSTEMS=="platform", SUBSYSTEM=="gpio", KERNEL=="gpiochip*", MODE="0660", GROUP="gpio" 
# https://docs.linuxkernel.org.cn/userspace-api/gpio/sysfs.html

# export 归属和权限： 匹配的是 gpiochipN
SUBSYSTEM=="gpio", KERNEL=="gpiochip*", PROGRAM="/bin/sh -c '\  
	chown root:gpio /sys/class/gpio/export /sys/class/gpio/unexport && \
	chmod ug+rw     /sys/class/gpio/export /sys/class/gpio/unexport '"  
# gpio操作 归属和权限:  匹配的是 gpioN
SUBSYSTEMS=="gpio", KERNELS=="gpiochip*", SUBSYSTEM=="gpio", KERNEL=="gpio*", PROGRAM="/bin/sh -c '\
	chown root:gpio /sys%p /sys%p/active_low /sys%p/direction /sys%p/edge /sys%p/value /sys%p/uevent && \
	chmod ug+rw     /sys%p /sys%p/active_low /sys%p/direction /sys%p/edge /sys%p/value /sys%p/uevent '"

# 如果认为写的粗糙，可以编写shell脚本实现。
```
 
### PWM files
pwm3-m0    pwm14-m1
 fd8b 0030   febf 0020
pwmchip1	pwmchip3
```bash
ls  /sys/class/pwm/pwmchip1
device  export  npwm  power  pwm0  subsystem  uevent  unexport

ls  /sys/class/pwm/pwmchip1/pwm0
capture  duty_cycle  enable  period  polarity  power  uevent
```

### PWM  udevadm info

```bash
udevadm info --attribute-walk --path=/sys/class/pwm/pwmchip1/pwm0

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
    
udevadm info --attribute-walk --path=/sys/class/pwm/pwmchip1

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
---
但是！ pwm的%p 定位有些许不同！
orangepi@orangepi5b:~$ echo 35 > /sys/class/gpio/export
orangepi@orangepi5b:~$ echo 35 > /sys/class/gpio/unexport
-
UDEV  [9786.070546] remove   /devices/platform/pinctrl/fec20000.gpio/gpiochip1/gpio/gpio35 (gpio)
KERNEL[9794.054738] add      /devices/platform/pinctrl/fec20000.gpio/gpiochip1/gpio/gpio35 (gpio)
---
---
orangepi@orangepi5b:/sys/class/pwm/pwmchip1$ echo 0  > export
orangepi@orangepi5b:/sys/class/pwm/pwmchip1$ echo 0  > unexport
-
KERNEL[10394.252728] change   /devices/platform/fd8b0030.pwm/pwm/pwmchip1 (pwm)
UDEV  [10394.273277] change   /devices/platform/fd8b0030.pwm/pwm/pwmchip1 (pwm)
---
由此可见，udev并没有精准的像 GPIO 一样，将 %p 识别为  /devices/platform/fd8b0030.pwm/pwm/pwmchip1/pwm0 (pwm)
而是，最多将 %p 定位到 /devices/platform/fd8b0030.pwm/pwm/pwmchip1 (pwm)

当然。 udev的SUBSYSTEMS=="pwm", KERNELS=="pwmchip*", KERNEL=="pwm*" 这些匹配规则仍然生效，只是pwm的 %p 和 gpio的 %p 略有不同
```

### PWM rules
```bash
# SUBSYSTEM=="pwm", KERNEL=="pwmchip*", MODE="0660", GROUP="pwm" 
# export 归属和权限
SUBSYSTEM=="pwm", KERNEL=="pwmchip*", PROGRAM="/bin/sh -c '\
	chown root:pwm /sys%p/ /sys%p/export  /sys%p/unexport /sys%p/uevent && \
	chmod ug+rw    /sys%p/ /sys%p/export  /sys%p/unexport /sys%p/uevent '" 
# pwm操作 归属和权限	
SUBSYSTEMS=="pwm", KERNELS=="pwmchip*", KERNEL=="pwm*", PROGRAM="/bin/sh -c '\
	chown root:pwm -R /sys%p/  && \
	chmod ug+rw    -R /sys%p/  '"
	
# 如果认为写的粗糙，可以编写shell脚本实现。这里直接递归修改了， 因为 pwm的%p只是定位到了 /devices/platform/fd8b0030.pwm/pwm/pwmchip1 这一级目录
```

 

 


 