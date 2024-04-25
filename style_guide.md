quick document to refactor for later

# Style guide
## General Naming
- public variables: PascalCase
- public/private top level constant variables: MACRO_CASE
- private top level members: m_camelCase
- private local (including constant) variables: camelCase
- enums: PascalCase
- methods camelCase
- classes PascalCase
## Specific Naming
- subsystems: PascalCaseSubsystem.java
- commands: PascalCaseCommand.java
- abstract subsystem: PascalCaseSubsystemBase.java
## Structure
- subsystems folder
  - subfolders grouping related subsystems (e.g. drive)
- commands folder
  - subfolders grouping related subsystems (e.g. arm)
  - one `auto` folder for auto related commands
- auto folder for full auto routines (not PathPlanner)
- utils folder for (typically) reused code
- PathPlanner stuff same as in its own docs
