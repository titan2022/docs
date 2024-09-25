# Tank Autonomous

## Getting code

Clone the [titan2022/Training2024](https://github.com/titan2022/Training2024) repository if you haven't done so in the previous lesson. Afterwards, create a branch from the `auton-lesson-template` branch and call it `auton-lesson-<USERNAME>`.

## Structure

This branch should contain a `TankDriveSubsystem` with three public methods: `driveLeft(double)`, `driveRight(double)`, and `stop()`. Utilize these (look more into the file for more information) in your commands to control the drivebase. `Robot.java` should have a `TankDriveSubsystem` initialized as `drive` on the line 18.

There will also be a `TankControlCommand` and `MoveCommand`. `TankControlCommand` is the teleop control command that we covered in the previous lesson. `MoveCommand` is simple: it sets all four motors to specified output, with no end condition. This means that the command will run forever without a timeout, which you can call through `.withTimeout(double)`.

Now, there's also a new way to call commands: `SequentialCommandGroup`. Initialize a new `SequentialCommandGroup` and pass the commands you want to run in order (don't schedule commands inside the group, see example). Since the next command can only start when the first finishes, make sure to add a terminating statement like `.withTimeout(double)`. Schedule the command group inside `autonomousInit()`---this will be your autonomous routine. Final example of how to use these elements can be found there already.

## The Task

1. Create a 90 degree rotation command. This should be done in a similar fashion to `MoveCommand` but the opposite sides should drive in opposite directions but same speed. As we do not have any input measurements, rotate the robot based on the amount of time it takes to do a 90 degree rotation (you will have to guess this).
2. Use a combination of `MoveCommand` and you rotation command inside a `SequentialCommandGroup` with `.withTimeout(double)` statements to create an autonomous cycle.
3. Your goal is to make the robot complete a square.