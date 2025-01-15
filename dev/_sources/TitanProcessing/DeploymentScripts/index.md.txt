# Deployment scripts

## Dependencies

Titan-Processing's conda environment should already be set up on the target. The target should have conda installed via miniforge3.

The host (your laptop containing the code to be sent to the target) should have `ssh` and `sshpass`.

On Debian, Ubuntu and Debian derivatives, these can be installed with:
```bash
sudo apt install openssh-client sshpass
```

On openSUSE and openSUSE derivatives, these can be installed with:
```bash
sudo zypper in openssh-clients sshpass
```

## Local deployment scripts

These assume a Debian Bookworm or Ubuntu 22.04 target. They are run directly on the coprocessor.

```bash
# Autostart the example `detect_headless`
./scripts/host-orangepi/autostart.sh
# Build Titan-Processing
./scripts/host-orangepi/build.sh
# Disable autostart and go back to the desktop
./scripts/host-orangepi/no-autostart.sh
```

## Remote deployment scripts

The deployment scripts assume a computer running Debian Bookworm, with Titan-Processing already built and its source code in `~/Projects/Titan-Processing`, with username `titan` and a password which you may find in the scripts.

```bash
# Set up pushes to the Orange Pi at the location specified in credentials.sh, and create a git remote called orangepi
./scripts/host-orangepi/set-up-push.sh
# Push the current HEAD to the Orange Pi's branch __titan_deployment_staging and build
./scripts/host-orangepi/push-build.sh
# Autostart the example `detect_headless`
./scripts/host-orangepi/autostart.sh
# Reboot the Orange Pi so that the autostart goes into effect
./scripts/host-orangepi/reboot.sh
# Show the log for the titan2022-apriltag.service
./scripts/host-orangepi/log.sh
# Disable autostart and go back to the desktop
./scripts/host-orangepi/no-autostart.sh
```

### Specifying credentials

In `scripts/host-orangepi/credentials.sh`, put:

```bash
git_remote=orangepi
remote_hostname=10.0.0.159
username=pi
password=raspberry
```

Replace the variables with the appropriate values for your device.

If you are a member of FRC#2022, please ping @ethanc8 for the credentials.
