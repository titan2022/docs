# Finding cameras

There are three locations with camera devices:

* `/dev/video*` - these are randomly assigned
* `/dev/v4l/by-id/` - by the name of the camera (which is typically the manufacturer and model number)
* `/dev/v4l/by-path/` - by the USB port

You can use `ls -l /dev/v4l/by-id/` and `ls -l /dev/v4l/by-path/` to correlate the two; they are symlinks to `/dev/video*`.

Most cameras have two devices, typically only the first one works.

## Testing cameras

start a stream from v4l2 device with m-jpeg transcoded into h264 rtp/udp
```bash
gst-launch-1.0 v4l2src device=/dev/video0 ! image/jpeg ! decodebin ! videoconvert ! x264enc tune=zerolatency bitrate=8000 speed-preset=medium ! rtph264pay ! udpsink host=127.0.0.1 port=8081
```

test the stream (requires graphical environment)
```bash
gst-launch-1.0 udpsrc port=8081 caps="application/x-rtp, media=(string)video, clock-rate=(int)90000, encoding-name=(string)H264, payload=(int)96" ! rtph264depay ! decodebin ! videoconvert ! autovideosink
```

If the graphical environment and the device with the camera are different, use `udpsink host=<IP address of graphical environment>` instead of `udpsink host=127.0.0.1`.
