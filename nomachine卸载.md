
```
sudo systemctl stop nxserver.service
 
sudo apt-get remove nomachine
 
sudo rm -rf /etc/nomachine
sudo rm -rf /var/lib/nomachine
sudo rm -rf /var/log/nomachine
 
sudo apt purge nomachine

 
 
dpkg -l | grep nomachine
 
```
systemctl list-units --type=service | grep nx
## 安装 
```
wget https://web9001.nomachine.com/download/9.3/Arm/nomachine_9.3.7_1_arm64.deb
 
sudo dpkg -i nomachine_9.3.7_1_arm64.deb
 
```

日志：

```
orangepi@orangepiWJH:~/Desktop$ sudo tail -f /var/log/Xorg.0.log
[ 26615.663] (II) modeset(0): Output DP-1 disconnected
[ 26615.663] (WW) modeset(0): Unable to find connected outputs - setting 1024x768 initial framebuffer
[ 26615.663] (==) modeset(0): Using gamma correction (1.0, 1.0, 1.0)
[ 26615.663] (==) modeset(0): DPI set to (96, 96)
[ 26615.663] (II) Loading sub module "fb"
[ 26615.663] (II) LoadModule: "fb"
[ 26615.663] (II) Loading /usr/lib/xorg/modules/libfb.so
[ 26615.663] (II) Module fb: vendor="X.Org Foundation"
[ 26615.663]    compiled for 1.20.13, module version = 1.0.0
[ 26615.663]    ABI class: X.Org ANSI C Emulation, version 0.4
[ 26615.763] (==) modeset(0): Backing store enabled
[ 26615.763] (==) modeset(0): Silken mouse enabled
[ 26615.763] (II) modeset(0): Initializing kms color map for depth 24, 8 bpc.
[ 26615.763] (==) modeset(0): DPMS enabled
[ 26615.763] (WW) modeset(0): Option "DRI" is not used
[ 26615.763] (WW) modeset(0): Option "FlipFB" is not used
[ 26615.763] (WW) modeset(0): Option "NoEDID" is not used
[ 26615.763] (WW) modeset(0): Option "UseGammaLUT" is not used
[ 26615.763] (WW) modeset(0): Option "Rotate" is not used
[ 26615.764] (II) modeset(0): [DRI2] Setup complete
[ 26615.764] (II) modeset(0): [DRI2]   DRI driver: rockchip
[ 26615.764] (II) modeset(0): [DRI2]   VDPAU driver: rockchip
[ 26615.764] (II) Initializing extension Generic Event Extension
[ 26615.764] (II) Initializing extension SHAPE
[ 26615.764] (II) Initializing extension MIT-SHM
[ 26615.764] (II) Initializing extension XInputExtension
[ 26615.764] (II) Initializing extension XTEST
[ 26615.765] (II) Initializing extension BIG-REQUESTS
[ 26615.765] (II) Initializing extension SYNC
[ 26615.765] (II) Initializing extension XKEYBOARD
[ 26615.765] (II) Initializing extension XC-MISC
[ 26615.765] (II) Initializing extension SECURITY
[ 26615.765] (II) Initializing extension XFIXES
[ 26615.765] (II) Initializing extension RENDER
[ 26615.765] (II) Initializing extension RANDR
[ 26615.766] (II) Initializing extension COMPOSITE
[ 26615.766] (II) Initializing extension DAMAGE
[ 26615.766] (II) Initializing extension MIT-SCREEN-SAVER
[ 26615.766] (II) Initializing extension DOUBLE-BUFFER
[ 26615.766] (II) Initializing extension RECORD
[ 26615.766] (II) Initializing extension DPMS
[ 26615.766] (II) Initializing extension Present
[ 26615.767] (II) Initializing extension DRI3
[ 26615.767] (II) Initializing extension X-Resource
[ 26615.767] (II) Initializing extension XVideo
[ 26615.767] (II) Initializing extension XVideo-MotionCompensation
[ 26615.767] (II) Initializing extension SELinux
[ 26615.767] (II) SELinux: Disabled on system
[ 26615.767] (II) Initializing extension GLX
[ 26616.191] (EE) AIGLX error: Calling driver entry point failed
[ 26616.212] (II) IGLX: Loaded and initialized swrast
[ 26616.212] (II) GLX: Initialized DRISWRAST GL provider for screen 0
[ 26616.212] (II) Initializing extension XFree86-VidModeExtension
[ 26616.213] (II) Initializing extension XFree86-DGA
[ 26616.213] (II) Initializing extension XFree86-DRI
[ 26616.213] (II) Initializing extension DRI2
[ 26616.214] (EE) modeset(0): Failed to create pixmap
[ 26616.214] (EE)
Fatal server error:
[ 26616.214] (EE) failed to create screen resources(EE)
[ 26616.214] (EE)
Please consult the The X.Org Foundation support
         at http://wiki.x.org
 for help.
[ 26616.214] (EE) Please also check the log file at "/var/log/Xorg.0.log" for additional information.
[ 26616.214] (EE)
[ 26616.215] (EE) Server terminated with error (1). Closing log file.

```