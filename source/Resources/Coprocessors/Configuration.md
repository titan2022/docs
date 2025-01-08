# Configuration

These are a few common configuration changes you might need to make to a coprocessor.

## Pi-Apps

This is not normally needed on a coprocessor but is helpful if you need to use it as a desktop for any reason. It has a lot of useful apps for ARM64 devices.

```bash
wget -qO- https://raw.githubusercontent.com/Botspot/pi-apps/master/install | bash
```

## `zram`

`zram` allows you to use a compressed section of RAM as a swapfile, rather than using the slow sdcard.

```bash
wget -qO- https://raw.githubusercontent.com/Botspot/pi-apps/master/apps/More%20RAM/install | bash
```

To disable:

```bash
wget -qO- https://raw.githubusercontent.com/Botspot/pi-apps/master/apps/More%20RAM/uninstall | bash
```

## Networking

### Connecting to Wi-Fi from terminal

Show the available Wi-Fi networks:

```bash
nmcli dev wifi
```

Connect to IMSApublic (example):

```bash
nmcli dev wifi connect IMSApublic password IMSAfall24
```

### Specify preferred IP address

This will still obtain the IP address over DHCP (automatic IP address setting controlled by the router) but will tell the router that you prefer a certain address.

If you want to prefer the address `143.195.90.11` while on IMSApublic, you can run:

```bash
sudo nmcli con modify IMSApublic ipv4.addresses 143.195.90.11/24
```

## Autostart, pushing, etc

See the [Titan-Processing deployment scripts](https://github.com/titan2022/Titan-Processing/tree/main/scripts) for an example on how to do this.
