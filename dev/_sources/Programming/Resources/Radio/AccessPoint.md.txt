# Access point mode

See [the docs](https://frc-radio.vivid-hosting.net/getting-started/usage/programming-your-radio#team-access-point-mode) for more information.

## What is access point mode?

In access point mode, it will provide a 6GHz Wi-Fi connection that can be used by both driver stations and robots, mimicking the field radio setup. It must be poswered from wall power and not by battery due to US regulations.

## Setup

Follow the [configuration instructions](Configuration.md), except that you should install the practice access point firmware instead of the radio variant firmware.

## Standard configuration for FRC#2022

* Team number: 2022
* SSID: `FRC-2022-AP#-@` where # is a number and @ is `R1` for Red 1, `B1` for Blue 1, etc.
  * Do it in order - see [the list](index.md#Radios)
* Wi-Fi channel: 93
* Red VLANs: 10, 20, 30
* Blue VLANs: 40, 50, 60
