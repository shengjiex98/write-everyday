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
- To set up hibernation in Ubuntu, find out the UUID of the partition `swapfile` is on as well as its file offset (first number in `physical_offset`. In this case `4974592`). Then add these information to `/etc/default/grub` in the `GRUB_CMDLINE_LINUX_DEFAULT` line.

      $ findmnt -no UUID -T /swapfile
      4a59c6a7-ca54-4e24-a362-3eac83bfe226
      $ sudo filefrag -v /swapfile
      $ sudo filefrag -v /swapfile
      Filesystem type is: ef53
      File size of /swapfile is 8589934592 (2097152 blocks of 4096 bytes)
      ext:     logical_offset:        physical_offset: length:   expected: flags:
        0:        0..    6143:    4974592..   4980735:   6144:            
        1:     6144..    8191:    4982784..   4984831:   2048:    4980736:
        2:     8192..   10239:    4988928..   4990975:   2048:    4984832:
        3:    10240..   12287:    4997120..   4999167:   2048:    4990976:
      ........................................
      $ sudo vim /etc/default/grub
      # it should look something like 
      GRUB_CMDLINE_LINUX_DEFAULT="quiet splash resume=UUID=4a59c6a7-ca54-4e24-a362-3eac83bfe226 resume_offset=4974592"

- `/etc/systemd/sleep.conf` for delay between hibernate and sleep.
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
