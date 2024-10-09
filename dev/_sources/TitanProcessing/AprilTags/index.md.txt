# AprilTag Detection

## Reading Configuration

### Setup

Initiate a `ConfigReader` object without a constructor and call the `readFromFile(std::string path)` method with specified directory path with the config to load it.

```cpp
ConfigReader config;
config.readFromFile("./example/"); // Relative to project root directory but absolute paths work too
```

## Detection

TitanProcessing library provides an `ApriltagDetector` class which processes the specified stream and sends pose packets over UDP. Initialize `ConfigReader`, `NetworkingClient`, `PoseFilter`, and `Localizer` and pass these objects to the `ApriltagDetector` constructor. The first `int` argument specified is the index of `/dev/video*` feed. The second `bool` argument specified whether or not to open the stream with a window (opens through X11 by default).

```cpp

```

## Multithreading