# Common Optimization Issues

## NetworkTables

NT3 had several design issues that [...]

NT4 switched to a publisher-subscriber (pubsub), also known as observer, structure that eliminated overrun issues from NT3.

### SmartDashboard

The SmartDashboard API should only be used for quick debug statements and never be used in production due to `CommandScheduleLoopOverrun` errors. 

[Show data from VisualVM]

[TODO: make the following line a caption + put link]
To see how to profile WPIlib code with VisualVM, see [profiling tutorial](#).

The reason why statements like `SmartDashboard.putNumber(...)` consume significant amount of CPU time is because they initialize the object both on the roboRIO and on the NT server each frame before updating the value, even if it is not necessary to do more than once.

The correct way to replace SmartDashboard API is to use NT publishers. They only initialize NT objects at start and simply update the value every command loop. The update function does not take as much

[...]

Publishers will initialize an object on NetworkTables only once. 

## IO

### SignalLogger

### PathPlanner