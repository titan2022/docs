# Coordinate systems

![Coordinate systems](CoordinateSystems.svg)

There are three coordinate systems in use in the codebase. The WPILib coordinate system (`CoordinateSystem::WPILIB`) is used when reading from WPILib-provided config files and in communication with the roboRIO (WIP). The THREE.js coordinate system (`CoordinateSystem::THREEJS`) is used throughout most of the codebase and when communicating with Titan-Dashboard. The OpenCV coordinate system (`CoordinateSystem::OPENCV`) is used for the results of certain OpenCV functions, such as `cv::solvePnP`.

