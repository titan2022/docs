# Installation

If you are having trouble with installing any of the following, please feel free to reach out to veteran programming members on Discord.

## Git
Our choice of project-management software is Git. If you do not have it installed already, do so at [its official website](https://git-scm.com/download/win) (Select 32-bit if your computer was made before 2003). All the default options are fine, feel free to spam "next".

## GitHub CLI
Create a GitHub account (username doesn't matter) or use an existing one. Send your username in the "GitHub Username" thread in `#programming` on Discord so I can add you to our organization. Afterwards, you need to login on your laptop. There are many ways but my most recommended is [GitHub CLI](https://cli.github.com/).

## Git login
When you install both Git and GitHub CLI, open a terminal window (preferably PowerShell) and type `gh auth login`. Select `Github.com`, `HTTPS`, `Y`, `Login with a web browser`, copy the 8 letter code and press enter. Paste your code on the web page and return back. You should be logged in.

## FRC Game Tools
The WPIlib suite is necessary to run robot code. Follow [instructions in the official docs](https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-2/frc-game-tools.html). It might say that you have a limited free trial but in reality it will never run out (similar to WinRAR). When choosing the VSCode option, select standalone install. Java will be included, but if you already have it installed it shouldn't interfere.

## Phoenix Tuner X
Phoenix Tuner X is the diagnostics tool for CTRE devices (e.g. Falcon 500 motors). It is [available on the Microsoft Store](https://apps.microsoft.com/detail/9nvv4pwdw27z).

## REV Hardware Client
REV Hardware Client is the REV-specific diagnostics tool for REV devices (e.g. SPARK MAX). Follow installation instructions on [its official page](https://docs.revrobotics.com/rev-hardware-client/gs/install).

## (Optional) Apache Maven
Install [Apache Maven](https://maven.apache.org/download.cgi) only if you are an experienced FRC programming member and wish to contribute to TitanAlgorithms. Make sure Maven is in your `PATH`.

## Linux
WPIlib support on Linux is unofficial and may not work correctly (use at your own risk). Follow installation instructions on [Ethan Charoenpitaks'es repository](https://github.com/ethanc8/FRCLinuxDevKit). More info regarding deployment can be found in [this thread](https://www.chiefdelphi.com/t/roborio-deployment-from-linux/452092)