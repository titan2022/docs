# 2 Command-based programming

(2.1)=
## 2.1 Lecture
[<big>**SLIDES**</big>](https://docs.google.com/presentation/d/1oYfl6V0sdiAbQPNuvmw6tMe7_MwqZdPDRTk7-M82QfE/edit)

### Further reading
* [*Command-Based Programming* in the WPILib documentation](https://docs.wpilib.org/en/stable/docs/software/commandbased/index.html)

(2.2)=
## 2.2 Activity

[slides](https://docs.google.com/presentation/d/19UBcB5TavPg54613Egh7rUXC6EmpprQ9OHRuUrDbzOI/edit?slide=id.g36a207f0c2e_0_88#slide=id.g36a207f0c2e_0_88)

### Specifications

You are given:

* A `Robot.java` with the `CommandScheduler` running  
* Skeleton for subsystems/commands

You will:

1. Populate `IntakeSubsystem.java`  
2. Add motor control (`runIn`, `runOut`, `stop`)  
3. Add a proximity sensor and Debouncer to detect a game piece  
4. Populate commands that:  
   * Run the intake until a game piece is detected (`IntakeUntilPiece`)  
   * Allow manual reverse (`IntakeOut`)  
5. Test using Gradle  
  ```bash
  ./gradlew cleanTest test --rerun-tasks
  ```

### Instructions

In your terminal, run 

```bash
git clone https://github.com/titan2022/command_based_project && cd command_based_project
```

Then run

```bash
./gradlew cleanTest test --rerun-tasks
```

This should throw a number of errors. You should complete the skeleton code according to the project specifications in the previous slide and rerun the test. It will result in passing (green) on all four tests once you have properly implemented everything. 

Step 1 -  Implement Motor
Step 2 - Add proximity sensor + Debouncer
Step 3 - Implement IntakeOut.java
Step 4 - Implement IntakeUntilPiece command
Step 5 - Add a manual reverse command (IntakeOut)

### Solution

The solution can be found in the `solution` branch of [@titan2022/command_based_project](https://github.com/titan2022/command_based_project/tree/solution).
