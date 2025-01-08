# Titan-Processing

## Introduction

[Titan-Processing](https://github.com/titan2022/Titan-Processing) is the official vision library created and used by Titan Robotics since the 2023-2024 season. It is targetted for compilation on a coprocessor, and for this reason it includes a networking module. The main capability is detecting AprilTags and determining the robot pose through localization and filtering it using an Extended Kalman Filter.

Unlike Titan Algorithms, Titan Processing operates on a rolling release model, meaning that there is no explicit versioning. This is because most of vision code in production is written in this library. This requires frequent bug fixes and modifications, so try to rebuild this library as often as possible. You can refer to specific Titan Processing installations with their respective Git commit messages or codes.

```{toctree}
---
maxdepth: 2
caption: Contents
titlesonly: true
---
Installation/index.md
Setup/index.md
AprilTags/index.md
Localization/index.md
Networking/index.md
DeploymentScripts/index.md
```

## Contributing

To contribute, create your own branch or fork with necessary modifications and initiate a merge or pull request into `main`.

## Other Tools
* [Camset](https://github.com/azeam/camset)
  * Quick way to calibrate camera parameters like exposure
* [calibration.py](Setup/Calibration.md)
  * Calibrating camera matrix and distortion coefficients
  * Use ChArUco board generator and print it
  * Take pictures with `[space]` (best to try multiple angles, 20+ photos)
* [Java networking server example](https://github.com/titan2022/Titan-Processing/blob/main/test/server/NetworkingServer.java)
  * UDP server for networking values
  * All types implimented
