# Orange Pi

We use "Orange Pi 5 Plus" with Ubuntu-Rockchip 22.04.

## Configuration

See also the [general configuration](Configuration.md)

### Device tree overlays

Device tree overlays can be enabled and disabled by editing `/etc/default/u-boot`. For example, we set:

```sh
## /etc/default/u-boot - configuration file for u-boot-update(8)

#U_BOOT_UPDATE="true"

#U_BOOT_ALTERNATIVES="default recovery"
#U_BOOT_DEFAULT="l0"
#U_BOOT_PROMPT="1"
#U_BOOT_ENTRIES="all"
#U_BOOT_MENU_LABEL="Debian GNU/Linux"
#U_BOOT_PARAMETERS="ro earlycon"
#U_BOOT_ROOT=""
#U_BOOT_TIMEOUT="50"
#U_BOOT_FDT=""
#U_BOOT_FDT_DIR="/lib/firmware/"
U_BOOT_FDT_OVERLAYS="device-tree/rockchip/overlay/orangepi-5-plus-disable-leds.dtbo"
#U_BOOT_FDT_OVERLAYS_DIR="/lib/firmware/"
#U_BOOT_SYNC_DTBS="false"
```

The device tree overlay `orangepi-5-plus-disable-leds.dtbo` disables the blue "heartbeat" LED, as it's annoying and can be distracting while programming. The available device tree overlays can be viewed using `ls /lib/firmware/6.1.0-*-rockchip/device-tree/rockchip/overlay/ | grep orangepi-5-plus`.

In order to do this, run:

```bash
sudo nano /etc/default/u-boot # Edit the file
sudo u-boot-update # Apply your changes
sudo reboot
```
