# Style guide

This guide aims to keep our team's code style consistent, so please follow it as much as possible. Access casing for variables and methods is especially important since a lot of code references code that someone else wrote. 

## Naming

### Java
* Public variables: PascalCase
* Public/private top level constant variables: `MACRO_CASE`
* Private top level members: `m_camelCase`
* Private local (including constant) variables: `camelCase`
* Enums: `PascalCase`
* Methods: `camelCase`
* Classes: `PascalCase`
* Class files: `PascalCase.java`
* Method files: `camelCase.java`
* Test files: `PascalCaseTest.java`
* Subsystem classes and files: `PascalCaseSubsystem.java`
* Abstract subsystem classes and files: `PascalCaseSubsystem.java`
* Command classes and files: `PascalCaseCommand.java`

### C++
* Public variables: `PascalCase`
* Public/private top level constant variables: `MACRO_CASE`
* Private top level members: `m_camelCase`
* Private local (including constant) variables: `camelCase`
* Enums: `PascalCase`
* Methods: `camelCase`
* Classes: `PascalCase`
* Class files: `PascalCase.cpp` or `PascalCase.hpp`
* Method files: `camelCase.cpp`
* Test files: `test_camelCase.cpp`
* CMake modules: `PascalCase.cmake`

## Structure

The following outlines the file structure in a WPILib projecct.

* `subsystems` folder
  * subfolders grouping related subsystems (e.g. drive)
* `commands` folder
  * subfolders grouping related subsystems (e.g. arm)
  * one `auto` folder for auto related commands
* Auto folder for full auto routines (not PathPlanner)
* `utils` folder for reused code
* Follow [PathPlanner documentation](https://pathplanner.dev/home.html) for PathPlanner structure
