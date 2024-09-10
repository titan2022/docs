# Installation

If you are having trouble with installing any of the following, please feel free to reach out to veteran programming members on Discord.

## Operating system

::::{tab-set}
:sync-group: os

:::{tab-item} Windows
:sync: win

Make sure you're on a 64-bit Windows version with an Intel or AMD CPU. All Windows 11 installations are 64-bit. If you're on Windows 10, you can check this in the Control Panel:

1. Click Start, type `system` in the search box, and, under Control Panel, click System.

```{note}
If System isn’t listed under Control Panel in the search results, click Control Panel to see more results.
```

2. Under System, look at the System type entry. It will say either 32-bit Operating System or 64-bit Operating System.
:::

:::{tab-item} Linux
:sync: linux

Make sure you're on a 64-bit (amd64 or arm64) distribution which uses glibc (which includes most popular distros such as Debian, Ubuntu, Mint, Arch, elementary OS, Fedora, Enterprise Linux, and SuSE).
:::

:::{tab-item} macOS
:sync: macos

Make sure you're on a recent macOS system. Both Intel Macs and Apple Silicon Macs are supported. Pre-2008 Macs are definitely not supported.
:::

::::

## Git
Our choice of project-management software is Git.

::::{tab-set}
:sync-group: os

:::{tab-item} Windows
:sync: win

If you do not have it installed already, do so at [its official website](https://git-scm.com/download/win) (select the 64-bit standalone installer). All the default options are fine, feel free to spam "next".
:::

:::{tab-item} Linux
:sync: linux

Download `git` from your distribution. If you don't know how to do that, please ask Ethan or Georgi.

On Debian, Ubuntu, Mint, and other distros that use `apt` or `apt-get`:
```bash
sudo apt install git
```
:::

:::{tab-item} macOS
:sync: macos

If you use Homebrew, run
```bash
brew install git
```

If you use MacPorts, run
```bash
sudo port install git
```

Otherwise, install [Homebrew](https://brew.sh/) and then follow the Homebrew instructions.
:::

::::


## GitHub CLI
Create a GitHub account (username doesn't matter) or use an existing one. Send your username in the "GitHub Username" thread in `#programming` on Discord so I can add you to our organization. Afterwards, you need to login on your laptop. There are many ways but my most recommended is [GitHub CLI](https://cli.github.com/).

## Git login
When you install both Git and GitHub CLI, open a terminal window (preferably PowerShell or Bash) and type `gh auth login`. Select `Github.com`, `HTTPS`, `Y`, `Login with a web browser`, copy the 8 letter code and press enter. Paste your code on the web page and return back. You should be logged in.

## (Only for pre-season) Java

::::{tab-set}
:sync-group: os

:::{tab-item} Windows
:sync: win

There are many JDKs (Java Development Kits) available online but [Temurin](https://adoptium.net/) is a great option. Choose OpenJDK 16 (LTS) and HotSpot JVM.
:::

:::{tab-item} Linux
:sync: linux

Your distribution should have a JDK available to download. If not, you can use WPILib's JDK by adding the following to your `~/.bashrc` or other shell configuration file:
```bash
export PATH="$HOME/wpilib/2024/jdk/bin/:$PATH"
export JAVA_HOME="$HOME/wpilib/2024/jdk/bin/"
```

If you're on Debian, Ubuntu, Mint, or some other `apt`-based distribution, you can install the system-recommended JDK using
```bash
sudo apt install default-jdk
```
:::

:::{tab-item} macOS
:sync: macos

You can use WPILib's JDK by adding the following to your `~/.bash_profile` and `~/.zprofile`:
```bash
export PATH="$HOME/wpilib/2024/jdk/bin/:$PATH"
export JAVA_HOME="$HOME/wpilib/2024/jdk/bin/"
```
:::

::::

## FRC Game Tools

::::{tab-set}
:sync-group: os

:::{tab-item} Windows
:sync: win

The WPIlib suite is necessary to run robot code. Follow [instructions in the official docs](https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-2/frc-game-tools.html). It might say that you have a limited free trial but in reality it will never run out (similar to WinRAR). When choosing the VSCode option, select standalone install. Java will be included, but if you already have it installed it shouldn't interfere.
:::

:::{tab-item} Linux
:sync: linux

Please use my script to install these tools. Make sure that `~/.local/bin` is on your `$PATH`.

To run the script, you must have a display (X11 or Wayland) and a network connection.

```bash
curl -o- -L https://raw.githubusercontent.com/ethanc8/FRCLinuxDevKit/master/install-online.sh | bash
```

If you also want to install the WPILib extension into your main VSCode or VSCodium installation, please set the environment variable FLDK_INSTALL_EXT_DESTINATION to the name of the command that launches VSCode (which is `code` for Microsoft binaries, `code-oss` if you compiled it yourself, and `codium` for VSCodium). For example:

```bash
curl -o- -L https://raw.githubusercontent.com/ethanc8/FRCLinuxDevKit/master/install-online.sh | FLDK_INSTALL_EXT_DESTINATION=code bash
```
:::

:::{tab-item} macOS
:sync: macos

This is **super experimental**, please tell Georgi and Ethan that you are trying this out if you do try it.

Please use my script to install these tools. Make sure that `~/.local/bin` is on your `$PATH`.

To run the script, you must have a display and a network connection.

```bash
curl -o- -L https://raw.githubusercontent.com/ethanc8/FRCLinuxDevKit/macos/install-online.sh | bash
```

If you also want to install the WPILib extension into your main VSCode or VSCodium installation, please set the environment variable FLDK_INSTALL_EXT_DESTINATION to the name of the command that launches VSCode (which is `code` for Microsoft binaries, `code-oss` if you compiled it yourself, and `codium` for VSCodium; you must be able to run this command from your shell first). For example:

```bash
curl -o- -L https://raw.githubusercontent.com/ethanc8/FRCLinuxDevKit/macos/install-online.sh | FLDK_INSTALL_EXT_DESTINATION=code bash
```
:::

::::

## Phoenix Tuner X

::::{tab-set}
:sync-group: os

:::{tab-item} Windows
:sync: win

Phoenix Tuner X is the diagnostics tool for CTRE devices (e.g. Falcon 500 motors). It is [available on the Microsoft Store](https://apps.microsoft.com/detail/9nvv4pwdw27z).
:::

:::{tab-item} Linux
:sync: linux

This is **completely optional**. It's a kind of involved process, and isn't useful unless you need to debug the motor controllers or other CTRE peripherals.

These instructions assume that you're on a Debian/Ubuntu system and are currently in a Wayland session.

First, install the Android rootfs and userspace libraries:

```bash
sudo apt install curl ca-certificates -y
curl https://repo.waydro.id | sudo bash
sudo apt install waydroid -y
sudo waydroid init
waydroid prop set persist.waydroid.multi_windows true
waydroid session stop
```

Now, download and install Phoenix Tuner X:

```bash
wget https://github.com/ethanc8/someblobs/releases/download/phoenix-tuner-x-2024.6.1.0/Tuner.X_2024.6.1.0_apkcombo.com.apk
waydroid app install "./Tuner X*.apk"
waydroid app launch com.ctre.phoenix_tuner
```
:::

:::{tab-item} macOS
:sync: macos

Unfortunately, Phoenix Tuner X is currently not available for macOS.
:::

::::

## REV Hardware Client

::::{tab-set}
:sync-group: os

:::{tab-item} Windows
:sync: win

REV Hardware Client is the REV-specific diagnostics tool for REV devices (e.g. SPARK MAX). Follow installation instructions on [its official page](https://docs.revrobotics.com/rev-hardware-client/gs/install).
:::

:::{tab-item} Linux
:sync: linux

Unfortunately, REV Hardware Client is currently not available for Linux.
:::

:::{tab-item} macOS
:sync: macos

Unfortunately, REV Hardware Client is currently not available for macOS.
:::

::::


## (Optional) Apache Maven
Install [Apache Maven](https://maven.apache.org/download.cgi) only if you are an experienced FRC programming member and wish to contribute to TitanAlgorithms. Make sure Maven is in your `PATH`.
