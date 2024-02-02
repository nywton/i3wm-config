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
