# Linux System Maintenance

## General

### Storage

- `du` ("disk usage", check files)
- `ncdu` ("ncurse du", an interactive command-line `du`)
- `df` ("disk free", check disk space)

### Battery
- Configuration file located at `/etc/tlp.conf`
- Use `systemctl start/stop/enable tlp.service` to start/stop/enable tlp
- Use `tlp-stat -b` to check battery status

### Sleep & Hibernation

- For ThinkPad laptops: set "Sleep Mode" in UEFI to `Linux`.

- `/etc/systemd/sleep.conf` for delay between hibernate and sleep

- Use `ln -s /usr/lib/systemd/system/systemd-suspend-then-hibernate.service /etc/systemd/system/systemd-suspend.service` to replace sleep with sleep-then-hibernate

- Create a `.rules` file like `/etc/udev/rules.d/logitech.rules` to disable device wake computer. To find the device ID use `lsusb` and the format will be `<idVendor>:<idProduct>`.

        ACTION=="add", SUBSYSTEM=="usb", DRIVERS=="usb", ATTRS{idVendor}=="046d", ATTR{power/wakeup}="disabled"
        # optionally
        ATTRS{idProduct}=="c52b"


### Matlab
- Create desktop entry

- [Remember last working folder](https://www.mathworks.com/matlabcentral/answers/336249-initial-working-folder-not-working#answer_326563)
  ```
  Exec=matlab -desktop -useStartupFolderPref

- Use correct video driver:
  ```
  Exec=env MESA_LOADER_DRIVER_OVERRIDE=i965 matlab -desktop
  ```

  

## Arch

### Pacman

- [Rosetta](https://wiki.archlinux.org/title/Pacman/Rosetta)
- Use `reflector` to manage mirror list.
- `systemd` env variables can be added to `~/.config/environment.d/envvars.conf` (used by Wayland)

## Ubuntu/Debian

### apt

- To list all repos: `$ egrep -v '^#|^ *$' /etc/apt/sources.list /etc/apt/sources.list.d/*`
