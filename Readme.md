# I3 Configuration

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
```

## Enabling bluetooth on Arch
```bash
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
