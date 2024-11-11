# i3wm-config
My personal settings and issues to i3wm on Ubuntu 22.04
## Installation
Ref: https://i3wm.org/docs/repositories.html
```
# for ubuntu 22.02
$ /usr/lib/apt/apt-helper download-file https://debian.sur5r.net/i3/pool/main/s/sur5r-keyring/sur5r-keyring_2023.02.18_all.deb keyring.deb SHA256:a511ac5f10cd811f8a4ca44d665f2fa1add7a9f09bef238cdfad8461f5239cc4
$ sudo apt install ./keyring.deb
$ echo "deb http://debian.sur5r.net/i3/ $(grep '^DISTRIB_CODENAME=' /etc/lsb-release | cut -f2 -d=) universe" | sudo tee /etc/apt/sources.list.d/sur5r-i3.list
$ sudo apt update
$ sudo apt install i3
```
## Config file
https://github.com/nywton/dotfiles/blob/main/i3wm/config
## Keyboard Settings
Used to US layout (used in Logitech K380)
```
$ setxkbmap -layout us -variant intl
```
### Set permannent keyboard layout
Some external keyboards (with Bluetooth connections) lose layout settings when the keyboard is disconnected.
There is a way to set it permanently by creating a Xorg#Configuration.
References: [Using X configuration files](https://wiki.archlinux.org/title/Xorg/Keyboard_configuration#:~:text=option%20grp%3Awin_space_toggle-,Using%20X%20configuration%20files,-Note%3A)
```
# Create the config file with sudo
$ sudo nvim /etc/X11/xorg.conf.d/00-keyboard.conf

# Add the layout and variant as following:
# /etc/X11/xorg.conf.d/00-keyboard.conf
Section "InputClass"
    Identifier "keyboard"
    MatchIsKeyboard "on"
    Option "XkbLayout" "us"
    Option "XkbVariant" "intl"
EndSection

# Restart Xorg: To apply the changes, you should restart the X server or simply reboot your system:

$ sudo systemctl restart display-manager.service
```

#### Display Arrangement (Hotplug Monitors)
References: [i3, udev, & xrandr: Hotplugging & Output Switching](https://frdmtoplay.com/i3-udev-xrandr-hotplugging-output-switching/)
```
# For external monitor above
$ xrandr --output HDMI-1 --auto --above eDP-1
```
# default monitor lg ultrawide
exec --no-startup-id xrandr --output HDMI-A-0 --mode 3440x1440 --rate 160
Detecting monitor switch events with `udevadm`.
```
$ sudo udevadm monitor

monitor will print the received events for:
UDEV - the event which udev sends out after rule processing
KERNEL - the kernel uevent


KERNEL[3959.208769] change   /devices/pci0000:00/0000:00:03.0/0000:01:00.0/drm/card0 (drm)
UDEV  [3960.654344] change   /devices/pci0000:00/0000:00:03.0/0000:01:00.0/drm/card0 (drm)

# Plugging/unplugging a HDMI/VGA/DisplayPort cable will cause events to fire and be printed to the terminal.
```

## Bluetooth
- Video Tutorial: https://www.youtube.com/watch?v=rOL-T31l0lQ
To enable bluetooth support in i3, you need to install the `bluez` package.
## Bluetooth UI in i3
run `blueman` for bluetooth grafical interface

## Enabling bluetooth on Arch
```bash
# install bluez
$ sudo pacman -S bluez bluez-utils

# checks if the btusb kernel module is loaded on your Linux system.
$ lsmod | grep btusb
# btusb                  77824  0
# btrtl                  32768  1 btusb
# btintel                69632  1 btusb
# btbcm                  24576  1 btusb
# btmtk                  32768  1 btusb
# bluetooth            1093632  27 btrtl,btmtk,btintel,btbcm,bnep,btusb

# if not loaded use:
$ sudo modprobe btusb

# check bluetoth service status
$ sudo systemctl status bluetooth

# enable bluetooth service (keep it alive after reboot)
$ sudo systemctl enable bluetooth.service

# start bluetooth service
$ sudo systemctl start bluetooth.service
```

## Using bluetothctl commands to control bluetooth
```bash
# first run bluetoothctl
$ bluetoothctl
# once started, turn on bluetooth with:
[bluetooth]# power on

# prevents disabling bluetooth when inactive or rebooting with agent by typing:
[bluetooth]# agent on
[bluetooth]# default-agent

# list all paired devices
[bluetooth]# devices

# scan for devices
[bluetooth]# scan on

# stop scanning for devices
[bluetooth]# scan off

# connect to a device
[bluetooth]# connect 00:00:00:00:00:00

# disconnect from a device
[bluetooth]# disconnect 00:00:00:00:00:00

# pair a device
[bluetooth]# pair 00:00:00:00:00:00

# remove a device
[bluetooth]# remove 00:00:00:00:00:00

# disconnect all devices
[bluetooth]# disconnect
```

## Edit bluetooth configuration
```bash
# edit the bluetooth configuration
$ sudo vim /etc/bluetooth/main.conf

# Change the AutoEnable property to true to don't need to enable it manually on boot: 
# /etc/bluetooth/main.conf  
AutoEnable=true
```

## bluetooth audio
```bash
# install bluez-alsa
$ sudo pacman -S pulseaudio-bluetooth pulseaudio

# start pulseaudio-bluetooth
$ pulseaudio --start
$ sudo systemctl start pulseaudio-bluetooth
```

## grafical interface for control audio
```bash
# install pavucontrol
$ sudo pacman -S pavucontrol
```

## DNS
Work on issues so I disabled IPV6
```bash
sudo sysctl -w net.ipv6.conf.all.disable_ipv6=1
sudo sysctl -w net.ipv6.conf.default.disable_ipv6=1

# restart networking
sudo systemctl restart NetworkManager systemd-resolved
```
### Make it permanent
```bash
# Create the config file with sudo
$ sudo vim /etc/sysctl.d/99-sysctl.conf                

# Add the layout and variant as following:
# /etc/sysctl.d/99-sysctl.config
# Disable IPv6
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
# Restart Xorg: To apply the changes, you should restart the X server or simply reboot your system:

$ sudo sysctl --system

# 3. Disable IPv6 in NetworkManager Configuration
# https://wiki.archlinux.org/title/NetworkManager#disable_ipv6
$ sudo vim /etc/NetworkManager/conf.d/disable-ipv6.conf

# Add the layout and variant as following:
# /etc/NetworkManager/conf.d/disable-ipv6.config
[connection]
ipv6.method=ignore
```
