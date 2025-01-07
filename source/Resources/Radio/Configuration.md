# Radio configuration and upgrade

The radio configuration page is at <http://192.168.69.1/>. Unfortunately, some radios on older firmware versions do not respond. Try to connect to <http://10.0.1.1/> instead.

The firmware version is at the bottom of the configuration page. If there is no firmware version on the configuration page, the firmware version can be found at <http://10.0.1.1/status> or <http://192.168.69.1/status>.

**Any radio with firmware version less than 1.2.6 has bugs that can brick the radio. Upgrade the firmware as soon as possible.**

## Upgrade instructions

* Download firmware from [Vivid Hosting](https://frc-radio.vivid-hosting.net/miscellaneous/firmware-releases)
* Power on the radio
    * You can power it via PoE or from the robot
* Plug an Ethernet cable in to your laptop
* Plug the Ethernet cable into the "DS" (driver station) port of the radio
* Go to the radio configuration page (<http://192.168.69.1/> or <http://10.0.1.1/>)
* Upload the firmware file to the configuration page (see [the Vivid Hosting docs](https://frc-radio.vivid-hosting.net/miscellaneous/upgrading-firmware) for more info)

