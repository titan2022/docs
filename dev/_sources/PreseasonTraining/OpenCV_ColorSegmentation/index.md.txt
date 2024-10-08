# Color-based segmentation in OpenCV Python

## Installation

Install Python (>=3.8) and the following packages using pip. You can install OpenCV with Python bindings another way, `opencv-python` is provided for simplicity of installation.
```bash
pip3 install opencv-python
pip3 install numpy
```

## Overivew

### Segmentation

In computer vision, image segmentation is the partitioning of the frame into several groups of pixels. Each segment typically represents a separate entity. Common application is separating a specific object from the background---a method of object detection. For this lesson, we will attempt image segmentation in Python OpenCV using colors.

Color can be represented in various formats, most common of which in software is Red Green Blue (RGB). Although it is convinient as monitors take RGB as input, it is not ideal for human interaction. For this, hue-based representations such as Hue Saturation Value (HSV) are more appropriate. For this lesson, we will use specific hue ranges to segment our frame.

### OpenCV

OpenCV is the largest computer vision library with an extensive set of features, meaning that there will always be multiple ways to achieve something. For this lesson, we will utilize OpenCV for our camera stream, access to data, and conditional segmentation.

### NumPy

NumPy is one of the most commonly used Python libraries. Its main purpose is to be an extended math module so you will have to use it often with other libraries. To note, the its types (Python has types but they're hidden and implied) are different from the standard types so you will have to initiate the NumPy specific types over the standard ones. Some types will be automatically converted but it is best to specify NumPy, as you will see later in this guide.

### Python

Although we have not covered Python in our pre-season training, it is designed to not be difficult to use. Unlike Java, Python does not have explicit types and you will not be required to write them. The biggest differences will be the syntax and the standard library. Look up information online (I recommend [W3Schools](https://www.w3schools.com/python/)) or ask others for help.

To run a Python program in Visual Studio Code, you can use the same "Run" button or shortcuts. Unlike Java, there is no explicit structure needed so you will have to have the file you're working on opened.

## The problem

### Instructions

Use the following info to help you achieve image segmentation using hue. Your goal is to create and calibrate a program that draws an outline accross your segmented object. Choose something bright with a unique hue and calibrate your hue ranges for it.

### Importing packages

To import pacakges in Python, we use the `import` keyword. You can also use `as` to shorten the name for readibility.

```py
import numpy as np # NumPy
import cv2 as cv   # OpenCV
import math        # Standard math module
```

### Camera stream

To get frames from your webcam, you need to "open" the stream. This means claiming the stream to this program and reading the data using the capture object in OpenCV. The first integer argument is the index of the camera. Unless you have multiple connected, it should be zero. Almost all OpenCV code starts with this template.

```py
cap = cv.VideoCapture(0)

while cap.isOpened():
    # Get frame
    _, frame = cap.read()

    # TODO: Do all your things with the frame here

    # Quit if Q is pressed (necessary, otherwise you would have to kill the program externally)
    if cv.waitKey(1) == ord('q'):
        break

# Close window if `cap.isOpened()` is false, which typically occurs after an error
cap.release()
cv.destroyAllWindows()
```

### Showing image

Use the `cv.imshow` function to show image with the first argument corresponding to window title and the second being the frame. Make sure this is inside your while loop so it keeps updating.

```py
cv.imshow("Window Title", your_frame)
```

### NumPy arrays

In Python, you can initiate an array using this syntax:

```py
arr = [1, 2, 3]
```

For NumPy arrays, use the `np.array` function:

```py
np_arr = np.array([1, 2, 3])
```

This is pretty much as much NumPy as you would need to know in this lesson.

### Converting frames

Here are some functions useful for converting frames from the standard BGR. See more at [this page](https://docs.opencv.org/3.4/d8/d01/group__imgproc__color__conversions.html).

```py
new_frame = cv.cvtColor(frame, cv.COLOR_BGR2RGB)  # BGR to RGB
new_frame = cv.cvtColor(frame, cv.COLOR_BGR2GRAY) # BGR to Grayscale
new_frame = cv.cvtColor(frame, cv.COLOR_BGR2HSV)  # BGR to HSV
```

### Masking

In graphic design and computer vision, masking is the action of hiding unnecessary part of an image. In a sense, it is image segmentation in our program. You can create a color based mask using this function.

```py
mask = cv.inRange(your_frame, lower_threshold, upper_threshold)
```

Where `your_frame` has to be an OpenCV frame, `lower_threshold` and `upper_threshold` have to be a 3-sized `np.array`. This will return an image where every included pixel is white (full values) and every excluded is black (zero values).

### Contours

To visualize our mask on top of the existing imaage, we can get the countours of our mask and draw them on top of the original image. The `cv.drawContours` function's first two arguments are your frame and contours respectively, the fourth argument is your RGB color of the line, and the last argument is the line width.

```py
contours, _ = cv.findContours(mask, cv.RETR_EXTERNAL, cv.CHAIN_APPROX_SIMPLE)
cv.drawContours(your_frame, contours, -1, (0, 255, 0), 3)
```