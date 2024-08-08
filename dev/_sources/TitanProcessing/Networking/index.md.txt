# Networking

## Why?

WPILib offers [Network Tables](https://docs.wpilib.org/en/stable/docs/software/networktables/networktables-intro.html) as a communication standard for communicating between roboRIO and coprocessors. This networking library tries to mitigate delay and reconnection problems with Network Tables by utilizing UDP sockets (lowest level networking protocol).

### Limitations

UDP achieves its performance by not accounting for lost packets. This limitation makes this library less reliable for one-time signals, but good enough for constant streams of data such as updating positions or video. Future ideas include utilizing TCP on top of UDP stream for solving this problem.

## C++ Client

### Setup

First, import `Client.h` header from `include/networking` directory in Titan Processing (temporary solution). Then create a `NetworkingClient` object with the IP and port of the roboRIO server. See which ports are allowed for use in the game manual or the [official documentation page](https://docs.wpilib.org/en/stable/docs/networking/networking-introduction/networking-basics.html).

```cpp
NetworkingClient client("10.20.22.9", 5800);
```

### Sending Information

You have to specify the label and send data using the following available types. More data types have to be manually implimented in the library.

```cpp
// Vectors
Vector3D vec(0.0, 0.0, 0.0);
client.send_vector("vector_name", vec, false);

// Pose (two vectors)
Vector3D position(0.0, 0.0, 0.0);
Vector3D rotation(0.0, 0.0, 0.0);
client.send_vector("pose_name", position, rotation);

// Tag (pose with id)
int id = 5;
Vector3D position2(0.0, 0.0, 0.0);
Vector3D rotation2(0.0, 0.0, 0.0);
client.send_tag("tag_name", 5, position2, rotation2);
```

### Sending Information (With Reply)

Same as before but specify `true` as second argument. This will return a vector object. Currently return statements are not supported as they are not tested. Only the vector type has a return implimentation.

```cpp
Vector3D reply = client.send_vector("vector_name", vec, true);
```

Receiving a reply is the only way to get data back from the server due to server-client limitations. You can reutilize the array to send any information outside of 3D vectors.

## C Client

Follow the same setup as with the C++ client, since the same header includes C bindings. All methods and types are replaced. The C client was intended to only be used as Python bindings.

```cpp
TRBNetworkingClientRef client = TRBNetworkingClientCreate("10.20.22.9", 5800);

// Vectors
TRBVector3D vec = TRBVector3DMake(0.0, 0.0, 0.0);
TRBVector3D returnVec = TRBNetworkingClientSendVector(client, "vector_name", vec, true);

// Pose (two vectors)
TRBVector3D position = TRBVector3DMake(0.0, 0.0, 0.0);
TRBVector3D rotation = TRBVector3DMake(0.0, 0.0, 0.0);
TRBNetworkingClientSendPose(client, "pose_name", position, rotation);

// Tag (pose with id)
int id = 5;
TRBVector3D position2 = TRBVector3DMake(0.0, 0.0, 0.0);
TRBVector3D rotation2 = TRBVector3DMake(0.0, 0.0, 0.0);
TRBNetworkingClientSendTag(client, "tag_name", 5, position2, rotation2);
```

## Python Client

### Setup

Reference `example/networking/TRBNetworking.py` file to your project and import the `Client` class from it. Then initialize the `Client` object

```python
client = Client("10.20.22.9", 5800)
```

### Sending Information

You have to specify the label and send data using the following available types. More data types have to be manually implimented in the library.

```python
# Vectors
vec = Vector3D(0.0, 0.0, 0.0);
client.sendVector("vector_name", vec, False);

# Pose (two vectors)
position = Vector3D(0.0, 0.0, 0.0);
rotation = Vector3D(0.0, 0.0, 0.0);
client.sendVector("pose_name", position, rotation);

# Tag (pose with id)
id = 5;
position2 = Vector3D(0.0, 0.0, 0.0);
rotation2 = Vector3D(0.0, 0.0, 0.0);
client.sendTag("tag_name", 5, position2, rotation2);
```

### Sending Information (With Reply)

Same as before but specify `True` as second argument. This will return a `Vector3D` object. Currently return statements are not supported as they are not tested. Only the vector type has a return implimentation.

```cpp
reply = client.sendVector("vector_name", vec, True);
```

Receiving a reply is the only way to get data back from the server due to server-client limitations. You can reutilize the array to send any information outside of 3D vectors. 

NOTE: Although Python does not explicitly show the type, all numerical values are converted to C doubles.

## Java Server

### Setup

Copy the `util` folder in `midwest` branch in `FRC-2024-JAVA` into your project (temporary solution). Then, initiate the `NetworkingServer` object in `robot.java` or another file with specified port (leave blank for default of 5800)

```java
NetworkingServer server = new NetworkingServer(5801);
```

### Receiving Information

Networking server uses observer structure to receive information. Subscribe to the server by passing the label and a lambda function, which will be called with each receive, as arguments. You can return a `Translation3d` as a reply, or `null` to not respond

```java
server.subscribe("position", (NetworkingCall<Translation3d>)(Translation3d position) -> {
    System.out.println(position.toString());
    return null;
});

server.subscribe("pose", (NetworkingCall<NetworkingPose>)(NetworkingPose pose) -> {
    System.out.println(pose.toString());
});

server.subscribe("tag", (NetworkingCall<NetworkingTag>)(NetworkingTag tag) -> {
    System.out.println(tag.toString());
});
```