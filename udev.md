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


    KERNEL=="pwmchip1"
    SUBSYSTEM=="pwm"