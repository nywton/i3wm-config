# i3wm-config
My personal settings and issues to i3wm on Ubuntu 22.04

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
