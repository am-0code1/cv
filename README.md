<!-- using https://github.com/FastLED/FastLED as the starting point -->
Particle Tracking (PT)
===
<!--
============
[![arduino-library-badge](https://www.ardu-badge.com/badge/FastLED.svg)](https://www.ardu-badge.com/FastLED)
[![build status](https://github.com/FastLED/FastLED/workflows/build/badge.svg)](https://github.com/FastLED/FastLED/actions/workflows/build.yml)
[![Documentation](https://img.shields.io/badge/Docs-Doxygen-blue.svg)](http://fastled.io/docs)
[![Reddit](https://img.shields.io/badge/reddit-/r/FastLED-orange.svg?logo=reddit)](https://www.reddit.com/r/FastLED/)
-->
<!-- [![Documentation](https://img.shields.io/badge/any_text-you_like-blue)](http://fastled.io/docs) -->
[![Documentation](https://img.shields.io/badge/Docs-Doxygen-blue)](http://fastled.io/docs)

Overview
---
This is a library for simulating motion of finite-size particles in fluid flow. The underlying algorithm takes into account interactions between particles and solid objects existing inside computational domain. The method includes a computationally-efficient, two-step search algorithm to mitigate the efficiency problems associated with the auxiliary structured grid (ASG) search algorithm when pinpointing hosting cell for a given location, in conjunction with a boundary condition to capture interactions between particle and solid objects.

Importance
---
Particle-wall interactions play a crucially important role in different applications such as microfluidic devices for
- cell sorting,
- particle separation,
- entire class of hydrodynamic filtration and its derivatives, etc.

Yet, accurate implementation of particle-wall interactions is not trivial when working with currently available algorithms/packages as they typically work with point-wise particles.

Current specifications
---
- **One-way coupling;** fluid flow is not affected by particles.
- **Hybrid unstructured mesh** consisting of triangular and/or quadrilateral cells can be processed.
- **Ansys mesh** can be processed.
- An **adaptive time-step integration** is implemented, wherein an appropriate time step is calculated based on local/instantaneous Stokes number of particle:
  - **Small Stokes regime**: particle is assumed to instantaneously follow local streamline of fluid flow by using one of the available integration schemes:
    - **Explicit Euler scheme**.
    - **Runge-Kutta**.
  - **Large Stokes regime:** The **velocity Verlet** is used to take into account **inertia effects** and to update instantaneous acceleration, velocity, and position of particle.
- **One or multiple particles** can be introduced in various fashions:
  - **point source** for one or multiple particles with various attributes, _e.g._, diameter, density, etc.
  - **streakline** for multiple particles with similar or various attributes, _e.g._, diameter, density, etc.
  - **arbitrary configurations** from a text file with various attributes, _e.g._, diameter, density, injection speed, etc.

What to expect
---
Here are multiple examples to give you some idea of the type of problems that can be addressed currently by using this library:

### Particle motion within microfluidic deterministic lateral displacement (DLD) device
Image of mesh

Image of particle trajectory

### Particle motion within microfluidic device with pinched flow
Image of mesh

Image of particle trajectory


> [!WARNING]
> The library has currently multiple limitations including:
> - It runs two-dimensional modeling. So, three-dimensional problems cannot be solved. 
> - It does not have a solver for fluid flow. Thus, fluid flow equations need to be solved, first, by using other packages, _e.g._, OpenFOAM, Ansys, COMSOL, etc., before using the library.
> - Only Ansys format is currently supported for mesh and fluid flow solution files.

Getting Started
---
Install the library using either [the .zip file from the latest release](https://github.com/FastLED/FastLED/releases/latest/) or by searching for "FastLED" in the libraries manager of the Arduino IDE. [See the Arduino documentation on how to install libraries for more information.](https://docs.arduino.cc/software/ide-v1/tutorials/installing-libraries)

How quickly can you get up and running with the library?  Here's a simple blink program:

```cpp
#include <FastLED.h>
#define NUM_LEDS 60
CRGB leds[NUM_LEDS];
void setup() { FastLED.addLeds<NEOPIXEL, 6>(leds, NUM_LEDS); }
void loop() {
	leds[0] = CRGB::White; FastLED.show(); delay(30);
	leds[0] = CRGB::Black; FastLED.show(); delay(30);
}
```

<!--
## Help and Support

If you need help with using the library, please consider visiting the Reddit community at https://reddit.com/r/FastLED. There are thousands of knowledgeable FastLED users in that group and a plethora of solutions in the post history.

If you are looking for documentation on how something in the library works, please see the Doxygen documentation online at http://fastled.io/docs.

If you run into bugs with the library, or if you'd like to request support for a particular platform or LED chipset, please submit an issue at http://fastled.io/issues.

## Supported LED Chipsets

Here's a list of all the LED chipsets are supported.  More details on the LED chipsets are included [on our wiki page](https://github.com/FastLED/FastLED/wiki/Chipset-reference)

* Adafruit's DotStars - aka APA102
* Adafruit's Neopixel - aka WS2812B (also WS2811/WS2812/WS2813, also supported in lo-speed mode) - a 3 wire addressable LED chipset
* TM1809/4 - 3 wire chipset, cheaply available on aliexpress.com
* TM1803 - 3 wire chipset, sold by RadioShack
* UCS1903 - another 3 wire LED chipset, cheap
* GW6205 - another 3 wire LED chipset
* LPD8806 - SPI based chipset, very high speed
* WS2801 - SPI based chipset, cheap and widely available
* SM16716 - SPI based chipset
* APA102 - SPI based chipset
  * APA102HD - Same as APA102 but with a high definition gamma correction function applied at the driver level.
* P9813 - aka Cool Neon's Total Control Lighting
* DMX - send rgb data out over DMX using Arduino DMX libraries
* SmartMatrix panels - needs the SmartMatrix library (https://github.com/pixelmatix/SmartMatrix)
* LPD6803 - SPI based chpiset, chip CMODE pin must be set to 1 (inside oscillator mode)

HL1606, and "595"-style shift registers are no longer supported by the library.  The older Version 1 of the library ("FastSPI_LED") has support for these, but is missing many of the advanced features of current versions and is no longer being maintained.

## Supported Platforms

Right now the library is supported on a variety of Arduino compatible platforms.  If it's ARM or AVR and uses the Arduino software (or a modified version of it to build) then it is likely supported.  Note that we have a long list of upcoming platforms to support, so if you don't see what you're looking for here, ask, it may be on the roadmap (or may already be supported).  N.B. at the moment we are only supporting the stock compilers that ship with the Arduino software.  Support for upgraded compilers, as well as using AVR Studio and skipping the Arduino entirely, should be coming in a near future release.

* Arduino & compatibles - straight up Arduino devices, Uno, Duo, Leonardo, Mega, Nano, etc...
* Arduino Yún
* Adafruit Trinket & Gemma - Trinket Pro may be supported, but haven't tested to confirm yet
* Teensy 2, Teensy++ 2, Teensy 3.0, Teensy 3.1/3.2, Teensy LC, Teensy 3.5, Teensy 3.6, and Teensy 4.0 - Arduino compatible from pjrc.com with some extra goodies (note the Teensy LC, 3.2, 3.5, 3.6, 4.0 are ARM, not AVR!)
* Arduino Due and the digistump DigiX
* RFDuino
* SparkCore
* Arduino Zero
* ESP8266 using the Arduino board definitions from http://arduino.esp8266.com/stable/package_esp8266com_index.json - please be sure to also read https://github.com/FastLED/FastLED/wiki/ESP8266-notes for information specific to the 8266.
* The wino board - http://wino-board.com
* ESP32 based boards

What types of platforms are we thinking about supporting in the future?  Here's a short list:  ChipKit32, Maple, Beagleboard

### Porting FastLED to a new platform

Information on porting FastLED can be found in the file
[PORTING.md](./PORTING.md).

## What about that name?

Wait, what happened to FastSPI_LED and FastSPI_LED2?  The library was initially named FastSPI_LED because it was focused on very fast and efficient SPI access.  However, since then, the library has expanded to support a number of LED chipsets that don't use SPI, as well as a number of math and utility functions for LED processing across the board.  We decided that the name FastLED more accurately represents the totality of what the library provides, everything fast, for LEDs.

## For more information

Check out the official site http://fastled.io for links to documentation, issues, and news


*TODO* - get candy
-->
