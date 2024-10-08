# Linux installation

2024 October 1/2

[Slideshow (intro to Linux)](https://docs.google.com/presentation/d/1lSk93V7GeIU5vTxyf7KXBiWZcuSrkg5RaqDZ4zDUYbg/edit#slide=id.p)

## Installing Debian

Download a Debian KDE live iso (we used <https://cdimage.debian.org/debian-cd/current-live/amd64/iso-hybrid/debian-live-12.7.0-amd64-kde.iso>, but there might be a newer version by the time you read this). Then, flash the iso onto your USB drive. On Linux, follow the instructions from the [Debian Live manual](https://live-team.pages.debian.net/live-manual/html/live-manual/the-basics.en.html#186), and on Windows, use the [Rufus app](https://rufus.ie/en/).

On the computer you want to install Debian onto, first make some free space in the Windows Disk Management (search for "Disk Management" in the Start menu). You'll want at least 50 GB of free space, but 100~200 GB is preferable.

Disable BitLocker on the disk (you can find guides on how to do this online). If you don't want to disable BitLocker, make sure you know the Microsoft account and password associated with the computer, and write down the BitLocker recovery key on paper or on another computer.

Plug in the USB stick with the Debian image. Reboot the computer, and spam the appropriate hotkey to open the BIOS settings as soon as the power light comes on. On most laptops, this is ESC, F2, or F10. In the BIOS, make the USB stick the default boot target, and disable Secure Boot (Secure Boot causes technical issues, since it prevents you from running any OS that has not been approved by Microsoft or someone Microsoft sold a signing key to). Then, save the BIOS changes.

Debian will ask you which session to boot into. Select the "Live system" using the Enter key and wait for it to boot. Open the installer, which should be on the desktop at the top left (but you can search for "Calamares" in the start menu or run `calamares` in the terminal if it's not there). Follow the installer's prompts. If the computer goes to sleep, the username for the live system is `live` and the password is also `live`.

After installation, you can reboot and remove the USB stick. It will ask you which operating system you want to boot -- you can use the arrow keys to select, and use Enter to boot the selected OS. If you wait too long, it will 

If you want to change the settings of the boot screen, boot into Debian, and use `sudo nano /etc/default/grub` to edit the settings file. Save the settings file with Ctrl-X, and then run `sudo update-grub` in order to merge the settings file with the other configuration files and apply it to the bootloader.
