# WifiEye

## Set up nexmon and nexmon_csi
**make sure kernel version is correct. Also will need corresponding kernel headers**. To check for kernel headers, look for `/lib/modules/$(uname -r)/build/`
To get a specific version of kernel, use [`rpi-update`](https://github.com/raspberrypi/rpi-update) with hashes from [`rpi-firmware`](https://github.com/raspberrypi/rpi-firmware). To get specific version of kernel headers, use [`rpi-source`](https://github.com/RPi-Distro/rpi-source).

## Use WifiEye server and studio

### Server
Need to use a `scan_wifi.sh` script or manual starting of nexmon_csi.

### Studio

Running studio requires qmake.
```bash
$ sudo apt-get install qtbase5-dev qtchooser qt5-qmake qtbase5-dev-tools
```
Fix the error in `studio`: Change `data` to `data[j]` `src/displayWidget.cpp:285:13`.