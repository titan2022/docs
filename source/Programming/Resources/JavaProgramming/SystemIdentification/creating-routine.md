% Sources
% https://docs.wpilib.org/en/stable/docs/software/advanced-controls/system-identification/creating-routine.html
% https://github.com/DeltaDizzy/deltas-docs-rewrites/blob/sysid-rewrite/docs/system-id/sysid/logging-data.md
% https://v6.docs.ctr-electronics.com/en/stable/docs/api-reference/wpilib-integration/sysid-integration/plumbing-and-running-sysid.html

# Creating an Identification Routine

## Types of Tests

A standard motor identification routine consists of two types of tests:

- **Quasistatic:** In this test, the mechanism is gradually sped-up such that the voltage corresponding to acceleration is negligible (hence, "as if static").
- **Dynamic:** In this test, a constant 'step voltage' is given to the mechanism, so that the behavior while accelerating can be determined.

Each test type is run both forwards and backwards, for four tests in total. The tests can be run in any order, but running a "backwards" test directly after a "forwards" test is generally advisable (as it will more or less reset the mechanism to its original position). `SysIdRoutine` provides command factories that may be used to run the tests, for example as part of an autonomous routine. Previous versions of SysId used a project generator to create and deploy robot code to run these tests, but it proved to be very fragile and difficult to maintain. The user code-based workflow enables teams to use mechanism code they already know works, including soft and hard limits.

## Units

Keeping track of units is one of the most critical tasks when doing System Identification. At all times, you *must* be aware of:

- The units your sensor reports in
- The units your motor controller needs setpoints in
- The units of the control gains

If the units do not match, some amount of manual dimensional analysis will be required to convert them.

## What to Log

SysId requires three different datasets for each motor that will be characterized:

1. Position
2. Velocity
3. *Output* Voltage (not battery/bus voltage)

## Choosing a Logger

SysId will only accept WPILib DataLogs (.wpilog files), but there are many ways to create them.

* Logging to NetworkTables and using `DataLogManager` to clone it to a DataLog
* Epilogue using a File backend
* DogLog (3rd-party library)
* Vendor logging solutions such as CTRE's `SignalLogger` or the `URCL` REV logger

When determining which logging solution to use, measurement timing is the primary consideration. The quality of the fit depends primarily on the level of timing jitter (or the amount that the time between measurements changes randomly during the test duration), though making the actual interval length itself smaller can also improve accuracy. Most timing jitter is caused by the Java Garbage Collector on the SystemCore, and so a low-hanging fruit for data improvement is to move to a lower-jitter collection method such as direct CAN timestamp measurements offtered by CTRE's `SignalLogger` or `URCL` for REV devices.

Any of these should work perfectly fine, but consult the documentation for each to learn how to use it. The remainder of this page will assume you are using WPILib logging.

## How to Log

:::{note}
For a full example project, see the SysIdRoutine example ([Java](https://github.com/wpilibsuite/allwpilib/tree/main/wpilibjExamples/src/main/java/edu/wpi/first/wpilibj/examples/sysidroutine), [C++](https://github.com/wpilibsuite/allwpilib/tree/main/wpilibcExamples/src/main/cpp/examples/SysIdRoutine), [Python](https://github.com/robotpy/examples/tree/main/SysId))
:::

WPILib provides the `SysIdRoutine` ([Java](https://github.wpilib.org/allwpilib/docs/release/java/edu/wpi/first/wpilibj2/command/sysid/SysIdRoutine.html)) class for creating and automating SysId tests to streamline data collection for teams. The API is designed with the Command-Based paradigm in mind, as it is used by the majority of FRC teams. The underlying `SysIdRoutineLog` recording API is usable in Non-Command codebases but will not be detailed here.

### SysIdRoutine

The intended usecase of `SysIdRoutine` is for one instance to be constructed in each subsystem. This instance must be provided with the test config (the Quasistatic test's voltage ramp rate, the Dynamic test's step voltage, and optionally a timeout) and a Mechanism object containing callbacks for driving the mechanism's motors and recording the test data as well as an instance of the subsystem object. If you do not understand Lambda Expressions or the concept of Callbacks, or just need a refresher, read the [Treating Functions as Data](https://docs.wpilib.org/en/stable/docs/software/basic-programming/functions-as-data.html) article.

The drive callback only needs to take the provided voltage object and use the voltage to set the motor output.

The data logging callback requires you to specify the name the motor will be logged under, as well as the values you wish to record. The API supports many quantities, but Voltage, Position, and Velocity are the important ones for System Identification purposes. Angular and linear position, velocity, and acceleration are distinct entities, so make sure you are using the correct units.

:::{note}
All datapoints must be passed to the API using unit-safe types (`Measure` in Java, an mp-units quantity in C++). Some motor APIs return data natively using unit-safe values, but the majority do not, so you will need to construct them using the motor data before or while calling the log methods!
:::

```java
SysIdRoutine routine = new SysIdRoutine(
    // no arguments means to use the defaults (1V / Second ramp rate, 7V step foltage, 10 second timeout).
    new SysIdRoutine.Config(
        Volts.of(0.5).Per(Second), // 0.5V / sec ramp rate
        Volts.of(4), // 4V step voltage
        Seconds.of(4) // 4 second timeout
    ), 
    new SysIdRoutine.Mechanism(
        // you can use a method reference for this, but if you understand lambda expressions it is easiest to use them instead.
        volts -> {
            motor.setVoltage(volts.magnitude());
        },

        log -> {
            // comment this
            log.motor("shooter-wheel")
                .voltage(Volts.of(motor.getVoltage()))
                .position(Rotations.of(motor.getPosition()))
                .velocity(Rotations.per(Second).of(motor.getVelocity()));
        }
    )
);
```

### SysIdRoutine details

#### Routine Config

The `Config` object takes in a a voltage ramp rate for use in Quasistatic tests, a steady state step voltage for use in Dynamic tests, a time to use as the maximum test duration for safety reasons, and a callback method that accepts the current test state (such as "dynamic-forward") for use by a 3rd party logging solution. The constructor may be left blank to default the ramp rate to 1 volt per second and the step voltage to 7 volts.

:::{note}
Not all 3rd party loggers will interact with SysIdRoutine directly. CTRE users who do not wish to use SysIdRoutine directly for logging should use the [SignalLogger](<https://pro.docs.ctr-electronics.com/en/latest/docs/api-reference/api-usage/signal-logging.html>) API and use Tuner X to convert to wpilog. REV users may use Team 6328's [Unofficial REV-Compatible Logger (URCL)](<https://docs.advantagescope.org/more-features/urcl>). In both cases the log callback should be set to `null`. Once the log file is in hand, it may be used with SysId just like any other.
:::

The timeout and state callback are optional and defaulted to 10 seconds and null (which will log the data to a normal WPILog file) respectively.

In order to use SysIdRoutine with a third-party logger, the 4th argument can be provided as follows (in this example we use CTRE's SignalLogger):

```{code-block} java
:emphasize-lines: 5
    new SysIdRoutine.Config(
        Volts.of(0.5).Per(Second), // 0.5V / sec ramp rate
        Volts.of(4), // 4V step voltage
        Seconds.of(4), // 4 second timeout
        (state) -> SignalLogger.writeString("state", state.toString())
    ), 
```

#### Declaring the Mechanism

The `Mechanism` object takes a voltage consumer, a log consumer, the subsystem being characterized, and the name of the mechanism (to record in the log). The drive callback takes in the routine-generated voltage command and passes it to the relevant motors. The log callback reads the motor voltage, position, and velocity for each relevant motor and adds it to the running log. The subsystem is required so that it may be added to the requirements of the routine commands. The name is optional and will be defaulted to the string returned by `getName()`.

The callbacks can either be created in-place via Lambda expressions or can be their own standalone functions and be passed in via method references. Best practice is to create the routine and callbacks inside the subsystem, to prevent leakage.

#### Mechanism Callbacks

The `Mechanism` callbacks are essentially just plumbing between the routine and your motors and sensors.

The `drive` callback exists so that you can pass the requested voltage directly to your motor controller(s).

The `log` callback reads sensors so that the routine can log the voltage, position, and velocity at each timestep.

See the SysIdRoutine ([Java](https://github.com/wpilibsuite/allwpilib/tree/main/wpilibjExamples/src/main/java/edu/wpi/first/wpilibj/examples/sysidroutine), [C++](https://github.com/wpilibsuite/allwpilib/tree/main/wpilibcExamples/src/main/cpp/examples/SysIdRoutine), [Python](https://github.com/robotpy/examples/tree/main/SysId)) example project for example callbacks.

### Test Factories

To be able to run the tests, SysIdRoutine exposes test "factories", or functions that each return a command that will execute a given test. These should be exposed by the subsystem.

```java
public Command sysIdQuasistatic(SysIdRoutine.Direction direction) {
  return routine.quasistatic(direction);
}

public Command sysIdDynamic(SysIdRoutine.Direction direction) {
  return routine.dynamic(direction);
}
```

Either bind the factory methods to either controller buttons or create an autonomous routine with them. It is recommended to bind them to buttons that the user must hold down for the duration of the test so that the user can stop the routine quickly if it exceeds safe limits.

Here is an example of binding buttons to these commands in `Robot` or `RobotContainer`:

```java
/*
 * Joystick Y = quasistatic forward
 * Joystick A = quasistatic reverse
 * Joystick B = dynamic forward
 * Joystick X = dyanmic reverse
 */
m_joystick.y().whileTrue(m_mechanism.sysIdQuasistatic(SysIdRoutine.Direction.kForward));
m_joystick.a().whileTrue(m_mechanism.sysIdQuasistatic(SysIdRoutine.Direction.kReverse));
m_joystick.b().whileTrue(m_mechanism.sysIdDynamic(SysIdRoutine.Direction.kForward));
m_joystick.x().whileTrue(m_mechanism.sysIdDynamic(SysIdRoutine.Direction.kReverse));
```

All four tests must be run and captured in a single log file. As a result, it is important that the user starts the logger before running the tests and stops the logger after all tests have been completed. (However, this is not standard practice when using loggers other than the CTRE Signal Logger.) This will ensure the log is not cluttered with data from other actions such as driving the robot to an open area. Here is an example for the CTRE Signal Logger:

```java
m_joystick.leftBumper().onTrue(Commands.runOnce(SignalLogger::start));
m_joystick.rightBumper().onTrue(Commands.runOnce(SignalLogger::stop));
```
