# Java / roboRIO Programming

```{toctree}
---
maxdepth: 2
caption: Contents
titlesonly: true
---
installation.md
```

## Introduction

The [official WPILib documentation](https://docs.wpilib.org/en/stable/) has the most amount of content regarding FRC programming. Reading through the entire Wiki will set you ahead of majority of FRC programmers. Some components do not have official WPILib support and contain their documentation externally, such as [Phoenix 6](https://v6.docs.ctr-electronics.com/en/stable/) and [NavX2](https://pdocs.kauailabs.com/navx-mxp/)

## Library guides

* WPILib
  * [Basic Programming](https://docs.wpilib.org/en/stable/docs/software/basic-programming/index.html)
  * [Command-Based Programming](https://docs.wpilib.org/en/stable/docs/software/commandbased/index.html)
  * [Kinematics and Odometry](https://docs.wpilib.org/en/stable/docs/software/kinematics-and-odometry/index.html)
* [CTRE Phoenix 6](https://v6.docs.ctr-electronics.com/en/stable/index.html)
* [PathPlanner](https://pathplanner.dev/home.html)

## API references

* [WPILib (`edu.wpilib.*`)](https://github.wpilib.org/allwpilib/docs/release/java/index.html) ([manual](https://docs.wpilib.org/))
    * Main FRC programming library
* [Phoenix 6 (`com.ctre.phoenix6.*`)](https://api.ctr-electronics.com/phoenix6/latest/java/) ([manual](https://v6.docs.ctr-electronics.com/en/latest/index.html))
    * Client library for CTRE devices
* [REVLib (`com.revrobotics.*`)](https://codedocs.revrobotics.com/java/com/revrobotics/package-summary.html) ([manual](https://docs.revrobotics.com/brushless/revlib/revlib-overview))
    * Client library for REV devices
* [PathPlannerLib (`com.pathplannerlib.*`)](https://pathplanner.dev/api/java/) ([manual](https://pathplanner.dev/pathplannerlib.html))
    * Runs PathPlanner-generated autos
* [Studica (`com.studica.frc.*`)](https://ethanc8.github.io/Studica-java-javadoc/) ([release notes](https://github.com/Studica-Robotics/NavX?tab=readme-ov-file#wpilib-vendordep))
    * Client library for Studica devices such as NavX
    * The API docs are unofficial and are for version 2025.0.0
