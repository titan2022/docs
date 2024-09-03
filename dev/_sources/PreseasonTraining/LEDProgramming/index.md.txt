# LED Programming

```{toctree}
---
maxdepth: 2
caption: Contents
titlesonly: true
---
Tutorial.md
```

## Setup

This lesson is about programming an LED strip using a microcontroller, such as an Arduino Uno. [Wokwi](https://wokwi.com/) is an online emulator that will substitute a real board.

In Arduino programming, you can can define two methods: `setup` and `loop`. The first runs once when the microcontroller turns on and the second repeats infinitely. For our purposes, `loop` is enough.

Here is a Wokwi instance that has the necessary code set up with 100 LED lights displaying a rainbow.

<iframe
  src="https://wokwi.com/projects/375725533336935425"
  style="width:100%; height:500px;"
></iframe>

FastLED, the library we are using for control, provides several functions. In this case, `FastLED.clear()` is necessary to set all pixels to an empty state. After setting the pixels, the board has to send the signal to the LED strip and you can do so through the `FastLED.show()` method.

Earlier in the Wokwi example I defined an array of 100 pixels called `leds`. You can access each pixel using the bracket operator and set them using the `CHSV(hue, saturation, value)` function. The library also provides a `CRGB(red, green, blue)` function if you want to set a color using the RGB standard. For both, all numbers are integers from 0 to 255 (2^8 or one byte).

```cpp
void loop() {
	FastLED.clear(); // Sets all pixels to blank

	leds[0] = CHSV(255, 255, 255); // Sets first pixel to red

	FastLED.show(); // Updates pixels
}
```

Here is another example that sets every pixel to red, instead of just the first one, utilizing a for loop.

```cpp
void loop() {
	FastLED.clear(); // Sets all pixels to blank

	for (int i = 0; i < NUM_LEDS; i++) {
		leds[i] = CHSV(255, 255, 255); // Sets i-th pixel to red
	}
	
	FastLED.show(); // Updates pixels
}
```

But to make it more interesting, you can utilize the `i` iterator variable in your hue calculations. Since `i` is an integer, it has to be converted into a decimal number (called `float` in most programming languages), only after which you can use it in division equations. Here we divide it by the number of LEDs to create a progression from 0.0 to 1.0 and then multiply by the hue range, that being 255. This for loop will go through every hue value from 0 to 255 and create a rainbow which you can see in the Wokwi example.

```cpp
leds[i] = CHSV((float)i / NUM_LEDS * 255, 255, 255);
```

But for now, our results are static. To animate, you can utilize the `delay(milliseconds)` function, similar to Python's `time.sleep(seconds)`. Without a delay, the board will repeat the `loop()` function as fast as it can.

Here is a list of some notable methods and features:
```cpp
// For loop (runs until second condition is false)
for (int i = 0; i < 100; i++) {
	// Do thing
}

// While loop (runs forever until condition is false)
int i = 0;
while (i < 100) {
	i++;
}

// If-else statement
if (thing1 == thing2) {
	// Do thing 1
} else {
	// Do thing 2
}

// The delay (sleep) funciton
delay(1000); // Waits 1 second

// Modulo operator
int number0 = thing % 2; // Returns remainder of "thing" divided by 2

/*

Math functions (specific to Arduino)
- sin(x)
- cos(x)
- tan(x)
- pow(x, y)
- sqrt(x)
- log(x) <-- natural log
- log10(x)

*/

leds[0] = CHSV(0, 0, 0); // Set color using HSV
leds[0] = CRGB(0, 0, 0); // Set color using RGB
```

## More examples

### Animated Rainbow

<iframe
  src="https://wokwi.com/projects/375728281107786753"
  style="width:100%; height:500px;"
></iframe>

### Sine Wave

<iframe
  src="https://wokwi.com/projects/375728506812760065"
  style="width:100%; height:500px;"
></iframe>

## Deploying

You can deploy your code on an actual Arduino Uno or another supported board by downloading desktop version of [Arduino IDE](https://www.arduino.cc/en/software) and creating a project. Connect your board, select it and the USB port under "Tools" tab. Click upload.