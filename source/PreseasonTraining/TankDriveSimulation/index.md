# Tank Drive Simulation

```{toctree}
---
maxdepth: 2
caption: Contents
titlesonly: true
---
Tutorial.md
```

## Getting code

Clone the [titan2022/Training2024](https://github.com/titan2022/Training2024) repository. Afterwards, create a branch from the `tank-drive-template` branch and call it `tank-drive-<USERNAME>`. All of the simulator code can be found in the `sim` folder, which you should not worry about or edit. Our editing entry point will be `src/main/java/frc/robot/Robot.java` (not the typical `Main.java`), which mimics the actual structure of a real Java FRC project.

## Structure

In FRC, the code structure consists of commands and subsystem classes.  On our physical robot, a group of components designed to do a certain function are also called a subsystem.

A (software) subsystem serves one purpose of only controlling one physical subsystem. Examples include:
* Driving subsystem: controls the drive base
* Arm subsystem: controls the arm
* Shooter subsystem: controls the shooting mechanism

A command serves one function and utilizes subsystems to do this function. Examples include:
* Intake command: picks up a game piece using the arm subsystem and the vision subsystem
* Balance command: balances the robot on a platform using the drive subsystem and the localization subsystem

This is called [Command Based Programming](https://docs.wpilib.org/en/stable/docs/software/commandbased/what-is-command-based.html). It is based on Java and C++’s [Object Oriented Programming](https://simple.wikipedia.org/wiki/Object-oriented_programming#Features) (OOP) pattern.

## FRC Programming

The main file is `Robot.java` . It contains these methods:

```java
public class Robot extends TimedRobot {
    @Override
    public void robotInit() { ... }
    @Override
    public void robotPeriodic() { ... }

    @Override
    public void autonomousInit() { ... }
    @Override
    public void autonomousPeriodic() { ...}

    @Override
    public void teleopInit() { ... }
    @Override
    public void teleopPeriodic() { ... }

    @Override
    public void disabledInit() { ... }
}
```

During FRC games, the first 15 seconds are for autonomous code. During this time, the players cannot control the robot. Afterwards for the rest of the match, the robot enters teleoperated mode and allows external control.`Init` methods are run once and `Periodic` methods are run every 20ms (50 times a second).

We will only work with teleop today (and most likely autonomous tomorrow!).

## Subsystems

Subsystems are almost empty classes. You can assign more methods and variables

```java
public class TankDriveSubsystem extends SubsystemBase {
		// Some variables here

		// The constructor
    public TankDriveSubsystem( ... ) {
        // Runs every time you create a new object with this class
				// Also processes the arguments
    }

		// Add more methods here
}
```
To use a subsystem, simply create a new object instance inside `Robot.java`
```java
public class Robot extends TimedRobot {
    private TankDriveSubsystem tankDrive = new TankDriveSubsystem();

    ...
}
```

## Commands

Commands require more specialized placing. You have to schedule the command inside `Init` methods of `Robot.java` only.

```java
@Override
public void teleopInit() {
    new TankControlCommand().schedule();
}
```

Each command will have four methods you can override:

### initialize()

```java
@Override
public void initialize() {
    // Runs once, runs first
}
```

### execute()

```java
@Override
public void execute() {
    // Runs every 20ms until command is finished
}
```

### end(boolean interrupted)

```java
@Override
public void end(boolean interrupted) {
    // Runs once when command is finished or interrupted
}
```

### isFinished()

```java
@Override
public boolean isFinished(boolean interrupted) {
    // Runs every 20ms alongside execute() but returns if the command is done or not
    return false;
}
```

## Xbox Controls

For our robot control we utilize the Xbox One controllers. In our simulator, we do not have enough controllers for everyone so you can use WASD (for left joystick), arrows (for right joystick), and X (for X button) keys on your keyboard.

> Typically you would need to create an `XboxController` object but our simulator has one, which you can access using `xbox` variable in `robot.java`.

To use it in a command, you have to pass the xbox object to the command in the constructor.

```java
new MyCommand(xbox).schedule();
```

Here are the methods of the controller:

```java
xbox.getLeftX() // Left joystick X-axis (from -1.0 to 1.0)
xbox.getLeftY() // Left joystick Y-axis (from -1.0 to 1.0)
xbox.getRightX() // Right joystick X-axis (from -1.0 to 1.0)
xbox.getRightY() // Right joystick Y-axis (from -1.0 to 1.0)
xbox.getXButton() // Returns true if X button is down
xbox.getXButtonPressed() // Returns true if X button just started a press
xbox.getXButtonReleased() // Returns true if X button just finished a press
```

## Motors

At our team we try to use the [Falcon 500](https://www.vexrobotics.com/217-6515.html) motors as much as we can. They include the motor itself and its motor controller (called Talon FX). We can access our motors through the `WPI_TalonFX` class and passing the ID of the motor to differentiate between them. It uses the following methods:

> This covers the [version 5](https://v5.docs.ctr-electronics.com/en/stable/) of CTRE Phoenix API, but we will be switching to [version 6](https://v6.docs.ctr-electronics.com/en/stable/) during the season

```java
WPI_TalonFX motor0 = new WPI_TalonFX(0, this); // Create a new motor with ID of 0
motor0.set(ControlMode.PercentOutput, 0.5); // Set motor to 50% of max output
motor0.set(ControlMode.Velocity, 0.5); // Set motor to 0.5 m/s speed
motor0.setInverted(true); // Change direction from clockwise to counter clockwise
```

The robot is pretty slow in our simulator, but in real life never set the motor anywhere close to full output or velocity!! Use percentages for this lesson since the values are easier to imagine.

## Extra

To debug and display values, we use `SmartDashboard` that sends numbers or strings from roboRIO to the driver station computer and displays them. Since we don't have a dashboard in this simulation, the result will be in the terminal. Here are the functions:

```java
SmartDashboard.putNumber("key", 5);
SmartDashboard.putBooleam("key", true);
SmartDashboard.putString("key", "value");
```

## The Task

For this lesson you have to create controls for a tank style drive base. It contains six wheels with four motors, two at each side. Motor IDs are: 0 and 1 on one side, 2 and 3 on the other. Motors that are together on a side will function identically. Use the xbox controller class to control the robot and move it around the screen. Feel free to use any configuration as far as the robot can move anywhere.

You have to figure out the rest of the information by playing around with the simulator. Feel free to ask me for help any time (including after the lesson). The criteria is:

- Use the xbox controller class
- Start from `Robot.java`
- Write the `TankDriveSubsystem.java` subsystem
- Write the `TankControlCommand.java` command
- The robot can move to any point on the screen
- You do not modify the contents of the `sim` folder or `Main.java` file