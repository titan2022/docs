# Tank Autonomous

2024 September 2024

## Getting code

Clone the [titan2022/Training2024](https://github.com/titan2022/Training2024) repository if you haven't done so in the previous lesson. Afterwards, create a branch from the `auton-lesson-template` branch and call it `auton-lesson-<USERNAME>`.

## Structure

This branch should contain a `TankDriveSubsystem` with three public methods: `driveLeft(double)`, `driveRight(double)`, and `stop()`. Utilize these (look more into the file for more information) in your commands to control the drivebase. `Robot.java` should have a `TankDriveSubsystem` initialized as `drive` on the line 18.

There will also be a `TankControlCommand` and `MoveCommand`. `TankControlCommand` is the teleop control command that we covered in the previous lesson. `MoveCommand` is simple: it sets all four motors to specified output, with no end condition. This means that the command will run forever without a timeout, which you can call through `.withTimeout(double)`.

Now, there's also a new way to call commands: `SequentialCommandGroup`. Initialize a new `SequentialCommandGroup` and pass the commands you want to run in order (don't schedule commands inside the group, see example). Since the next command can only start when the first finishes, make sure to add a terminating statement like `.withTimeout(double)`. Schedule the command group inside `autonomousInit()`---this will be your autonomous routine. Final example of how to use these elements can be found there already.

## Final code (from after the lesson)

[branch auton-lesson-template](https://github.com/titan2022/Training2024/tree/auton-lesson-template)

## The Task

1. discard all your changes and merge from `auton-lesson-template` (just create a new branch off of it) (your progress will be lost again, mb)
2. utilize `MoveCommand` and create a rotation command
- you only need to turn 90 degrees, so making it time based is fine
- copy from `MoveCommand` if needed
3. use `SequentialCommandGroup` to run commands in order
4. add `.withTimeout(X)` to terminate each command after `X` seconds
5. see existing `SequentialCommandGroup` example in the branch and ping me for help
6. use everything to make the robot complete a square
