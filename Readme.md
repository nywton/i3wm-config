# I3 Configuration

## Bluetooth
To enable bluetooth support in i3, you need to install the `bluez` package.
## Bluetooth UI in i3
run `blueberry` for bluetooth settings

## Enabling bluetooth on Arch
```bash
# check bluetoth service status
sudo systemctl status bluetooth

# enable bluetooth service (keep it alive after reboot)
sudo systemctl enable Bluetooth

# start bluetooth service
sudo systemctl start bluetooth
```
