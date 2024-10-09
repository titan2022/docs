# Color-based segmentation in OpenCV Python

2024 October 8/9

[Slideshow (color-based segmentation)](https://docs.google.com/presentation/d/1IMgoiuJn4d6MBo8bgtqWCU0rHaVIX8sB9S1IGK5x0JQ/edit?usp=sharing)

## Installation

On Windows, install Python 3.12 from the Microsoft Store. On Linux, Python should be preinstalled. Ping Georgi and Ethan if you need help.

Then install OpenCV:
```bash
pip install opencv-python opencv-contrib-python numpy
```

(there are also other ways to install it, but this is the simplest.)

## Overivew

### Segmentation

In computer vision, image segmentation is the partitioning of the frame into several groups of pixels. Each segment typically represents a separate entity. Common application is separating a specific object from the background---a method of object detection. For this lesson, we will attempt image segmentation in Python OpenCV using colors.

Color can be represented in various formats, most common of which in software is Red Green Blue (RGB). Although it is convinient as monitors take RGB as input, it is not ideal for human interaction. For this, hue-based representations such as Hue Saturation Value (HSV) are more appropriate. For this lesson, we will use specific hue ranges to segment our frame.

### OpenCV

OpenCV is the largest computer vision library with an extensive set of features, meaning that there will always be multiple ways to achieve something. For this lesson, we will utilize OpenCV for our camera stream, access to data, and conditional segmentation.

### NumPy

NumPy is one of the most commonly used Python libraries. Its main purpose is to be an extended math module so you will have to use it often with other libraries. To note, its types (Python has types but they're hidden and implied) are different from the standard types so you will have to initiate the NumPy specific types over the standard ones. Some types will be automatically converted but it is best to specify NumPy, as you will see later in this guide.

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

Use the `cv.imshow` function to show image with the first argument corresponding to window title and the second being the frame. Make sure this is inside your while loop so it keeps updating. The window will take in BGR, just like the camera feed, so you don't need to convert the stream to show it properly.

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

In graphic design and computer vision, masking is the action of hiding unnecessary part of an image. In a sense, it is image segmentation in our program. You can create a color based mask using this function. Note that all three values are the size of a byte, meaning the values should be unsigned integers from 0 to 255 inclusive.

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

## Calibration

### Intro

Since each camera has different physical properties and lenses, they need to be calibrated individually. More often than not, constants such as field of view of a specific webcam are not available online. For this reason, we use a calibration script that scans images of an AruCo chessboard with known dimentions to calculate these constants. For this lesson, you will only need to calibrate your camera for the distance challenge.

### Running

Download the `opencv-contrib-python` package using pip and run [this file](https://github.com/titan2022/Training2024/blob/calibration/calib.py) to calibrate your camera. The console output will show your X and Y field of view in degrees. More data such as the camera matrix and distortion coefficients are available in the script.

When running the program, show your AruCo chessboard (which should be taped to a flat surface) at various angles and press SPACE key to take a picture. Take approximately 10 to 20 pictures. Press ESC key to exit and copy the values printed to the console.

## Challenges

After completing your initial problem, feel free to try the following challenges to expand your program.

### Finding the center

For use in locating the specific object, its position in the frame needs to be obtained. The best representation of the position would be the center of the contour in pixels on the image frame. For this challenge, use the available contour data to calculate its center in pixels. The `contours` object is an array that contains each contour (often you might have multiple) in a form of another array of 2D vertices. Print it to the console or [read more about it online](https://docs.opencv.org/3.4/d4/d73/tutorial_py_contours_begin.html) to figure out the structure of the array and use the data points to calculate the center.

### Calculating object distance

Given that the object you want to detect is a near perfect sphere with constant radius, you can use trigonometry to calculate the distance of the sphere from the camera. Use the calibration script to get your field of view of your webcam and use all your known variables to calculate for depth. In a sense, you are trying to translate 2D pixel input (radius of circle in pixels) to 3D distance output. Read this [Wikipedia article](https://en.wikipedia.org/wiki/3D_projection) about perspective or search more online resources for help.