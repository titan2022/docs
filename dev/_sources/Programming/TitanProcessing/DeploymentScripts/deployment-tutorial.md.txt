# Deployment tutorial

## Setting up your laptop

### Titan-Processing

Boot into Linux, and download Titan-Processing if you haven't already.

```bash
mkdir -p ~/Projects; cd ~/Projects
git clone https://github.com/titan2022/Titan-Processing
cd Titan-Processing
```

If you already downloaded Titan-Processing, make sure to switch to the `wpimath` branch and pull the changes.

```bash
git fetch
git checkout wpimath
git pull
```

In `scripts/host-orangepi/credentials.sh` put the credentials for the Orange Pi you're deploying to in the file. Ignore the IP address for now. (If you're on FRC#2022, ping @ethanc8 for the credentials.)

### SSH, sshpass

On Debian, Ubuntu and Debian derivatives, these can be installed with:
```bash
sudo apt install openssh-client sshpass
```

On openSUSE and openSUSE derivatives, these can be installed with:
```bash
sudo zypper in openssh-clients sshpass
```

### Angry IP Scanner

Download [here](https://angryip.org/).

### Nmap

CLI and lighter alternative to Angry IP Scanner. Download [here](#).

## Wiring and networking

There are two options:

* Network sharing (tethering) - if you need to do first-time setup or update the packages on the Orange Pi
* Robot network - when the Orange Pi is already on the robot, and you don't need to do first-time setup or update packages

### Network sharing (tethering)

Plug an ethernet cable directly into both the Orange Pi and the laptop. Make sure the Orange Pi has no other network connections and the laptop has Internet via another Ethernet port, via WiFi, or some other way.

Then follow the instructions in [Network Sharing](/Resources/Networking/network-sharing).

### Robot network

Connect to the radio via Ethernet or WiFi (do not connect directly to the Orange Pi or to the roboRIO). Use either Angry IP Scanner or Nmap to find the IP of the Orange Pi.

Use Angry IP Scanner to scan the address range `10.TE.AM.0` to `10.TE.AM.255` (for FRC#2022 this is `10.20.22.0` to `10.20.22.255`). Find the Orange Pi in the list of devices in the range (its hostname should show up in another column; usually its IP is 10.20.22.200-10.20.22.210 but it can vary).

Alternatively use Nmap to scan the address range `10.TE.AM.0` to `10.TE.AM.255` using the following command,
```bash
nmap -sP 10.TE.AM.0/24
```
where the number `/24` (out of 32 bytes) indicates the subnet `255.255.255.0`. Although not applicable to this tutorial but, for example, the subnet `255.255.0.0` can be represented as `/16`. The flag `-sP` means skip port scan, meaning that it will not try to find open ports after finding a host, taking less time. The output will display a list of devices on the network. Simply copy the IP you see that is not your own.

## Next steps

1. Put the IP address you find into `scripts/host-orangepi/credentials.sh`.
2. SSH into the Orange Pi: (use the correct username and hostname)
   ```bash
   ssh pi@10.20.22.200
   ```
   Make sure to accept the Orange Pi's key fingerprint.
3. Ensure that you can use the Titan-Processing scripts to SSH:
   ```bash
   scripts/host-orangepi/ssh.sh
   ```

## First-time setup

You can skip this step if the Orange Pi was already set up with an older version of Titan-Processing.

1. Create an empty repo on the Orange Pi:
   ```bash
   scripts/host-orangepi/create-remote-repo.sh
   ```

   Alternately, you can clone Titan-Processing into `~/Projects/Titan-Processing`.
2. Set up pushes to the Orange Pi:
   ```bash
   scripts/host-orangepi/set-up-push.sh
   ```
3. Push the code to the Orange Pi:
   ```bash
   scripts/host-orangepi/push.sh
   ```
4. Install the dependencies on the Orange Pi:
   ```bash
   scripts/host-orangepi/rebuild-environment.sh
   ```

Continue into normal usage.

## Normal usage

1. If this is the first time you connect to this Orange Pi from this laptop, set up pushes:
   ```bash
   scripts/host-orangepi/set-up-push.sh
   ```

   If this isn't the first time, but the IP address of the Pi has changed since the last time you pushed, reset the IP address:
   ```bash
   scripts/host-orangepi/reset-push-ip.sh
   ```
   
   Otherwise, you should be ready for the next step.
2. Push the code to the Orange Pi and build it:
   ```bash
   scripts/host-orangepi/push-build.sh
   ```

   If you've already built the code and haven't cleaned it, you can just build the changes instead of rebuilding the whole thing:
   ```bash
   scripts/host-orangepi/push-fastbuild.sh
   ```

   Note that this sometimes results in issues.

### Updating the dependencies

Sometimes you might add more dependencies. In that case, you can add those dependencies to the Orange Pi using
```bash
scripts/host-orangepi/update-environment.sh
```

Sometimes the environment might get into an inconsistent state or use really outdated packages. In that case you can rebuild it using
```bash
scripts/host-orangepi/rebuild-environment.sh
```

### Autostarting

* Enable autostart and reboot:
  ```bash
  scripts/host-orangepi/autostart.sh
  scripts/host-orangepi/reboot.sh
  ```
* Disable autostart and reboot:
  ```bash
  scripts/host-orangepi/no-autostart.sh
  scripts/host-orangepi/reboot.sh
  ```

