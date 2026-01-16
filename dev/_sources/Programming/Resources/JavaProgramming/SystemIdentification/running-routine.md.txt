% Sources
% https://docs.wpilib.org/en/stable/docs/software/advanced-controls/system-identification/running-routine.html
% https://github.com/DeltaDizzy/deltas-docs-rewrites/blob/sysid-rewrite/docs/system-id/sysid/logging-data.md
% https://v6.docs.ctr-electronics.com/en/stable/docs/api-reference/wpilib-integration/sysid-integration/plumbing-and-running-sysid.html
% https://docs.frcteam3636.com/sysid

# Running the Identification Routine

Once the code has been deployed, we can now run the system identification routine, and record the resulting data for analysis.

:::{warning}
Only log files with a single routine in them are usable for analysis. Multiple motors can be run in one routine, but they must be run at the same time. If you run a routine on one motor and then run a routine on another motor without extracting the log or power-cycling the roboRIO in between, analysis will fail.
:::

## Before characterization

There are a couple of important things to consider before running the characterization tests.

**Characterization Can Be Dangerous:**

:::{danger}
Always use caution when mechanisms are moving and ensure that the robot can be disabled swiftly at any time!
:::

- Since characterization applies a scaling (quasistatic) or constant (dynamic) voltage to the motor, it can very easily hit a wall (drivetrain) or break the mechanism (elevator) if unprepared. Ensure that the ramp rate is set appropriately and adequate space is given for the tests.

**Ensure Adequate Space**

- If the mechanism is continuous (swerve azimuth or a flywheel), then this is not an issue. However, mechanisms such as a drivetrain or elevator have a limited degree of movement. Ensure the configuration parameters match what is possible, and be prepared to disable the robot early.
  - For the drivetrain characterization, the WPILib developers recommend 3-6 m and CTRE recommends 15 m of clear space.
  - The robot drive can not be accurately characterized while on blocks.
  - **Don't hit the hard stop**: Running the mechanism past the end of its range of motion might cause SysID to think the mechanism has more friction than it actually does.
  - **Slow it down:** If the routine is running too fast to record high-quality data, change the parameters passed to `SysIdRoutine.Config` to make it run slower.

**Only Run Each Test Once**

- Limitations of the SysId desktop utility prevent multiple of the same tests to be properly analyzed. Ensure each test is run exactly once.


## Running Tests
Perform the tests using the bindings you created in the previous section.

:::{warning}
Watch out for your mechanism and stop the test early if it exceeds safe limits! The routine only creates voltage commands for you to connect to your motors, it is up to you to set up hard or soft limits to prevent injury or damage.
:::


The quasistatic test will slowly ramp up voltage until the button has been released or a timeout has been hit. It is always safe to end the tests early, but at least ~3-5 seconds of data is necessary. Ensure ramp rate is configured such that this can be accomplished.

The dynamic test will immediately run the mechanism at the target voltage. This voltage may need to be adjusted if there is not sufficient room for the test.

With the routines configured and buttons set up, the characterization tests can be performed. To keep things simple and debuggable, perform tests in the following order.

1. Quasistatic forward
2. Quasistatic reverse
3. Dynamic forward
4. Dynamic reverse

Ensure each test is ran once, and only once. If a test is accidentally started multiple times, stop and restart the Signal Logger and try again.

The entire routine should look something like this:

:::{note}
A drivetrain routine is shown below, but the same motions will occur on any mechanism.
:::

```{raw} html
<div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;"> <iframe src="https://www.youtube-nocookie.com/embed/FN2xqoB1sfU" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe> </div>
```


## Extracting Logs

### WPILib logging

After recording your log, you must extract it from the roboRIO. WPILib logging will leave a .wpilog on the roboRIO, which can be downloaded remotely using `DataLogTool`. If a USB flash drive was plugged into the SystemCore and had sufficient free space, the log will have been recorded to it, so unplugging the drive and copying the file onto your computer is also possible.

After the log file is in hand, open the SysId application and proceed to [Loading Data](loading-data.md).

### Phoenix 6 Signal Logger


Once you have a log with all the tests, you can use Tuner X or the {ref}`owlet CLI tool <https://v6.docs.ctr-electronics.com/en/stable/docs/api-reference/api-usage/signal-logging.html#converting-signal-logs>` to {doc}`extract the hoot log to WPILOG <https://v6.docs.ctr-electronics.com/en/stable/docs/tuner/tools/log-extractor.html>`. The exported WPILOG can then be `loaded into SysId <https://docs.wpilib.org/en/stable/docs/software/advanced-controls/system-identification/loading-data.html>`__ for analysis using the Talon FX ``Position``, ``Velocity``, and ``MotorVoltage`` signals.

.. important:: We recommend users do **not** use third-party tools to export a ``hoot`` log to WPILOG. Doing so may result in a lossy conversion that impacts the quality of the SysId analysis. This is particularly true in simulation, where a lossy export can result in SysId failing to analyze the data.
