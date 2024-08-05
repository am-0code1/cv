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

# Overview
This is a library for simulating motion of finite-size particles in fluid flow. The underlying algorithm takes into account interactions between particles and solid objects existing inside computational domain. The method includes a computationally-efficient, two-step search algorithm to mitigate the efficiency problems associated with the auxiliary structured grid (ASG) search algorithm when pinpointing hosting cell for a given location, in conjunction with a boundary condition to capture interactions between particle and solid objects.

## Importance
Particle-wall interactions play a crucially important role in different applications such as microfluidic devices for
- cell sorting,
- particle separation,
- entire class of hydrodynamic filtration and its derivatives, etc.

Yet, accurate implementation of particle-wall interactions is not trivial when working with currently available algorithms/packages as they typically work with point-wise particles.

## Current specifications
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

## What to expect
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

# Getting Started
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

# How to contribute code

Follow these steps to submit your code contribution.

## Step 1. Open an issue

Before making any changes, we recommend opening an issue (if one doesn't already exist) and discussing your proposed changes. This way, we can give you feedback and validate the proposed changes.

## Step 2. Make code changes

To make code changes, you need to fork the repository.

## Step 3. Create a pull request
Once the change is ready, open a pull request from your branch in your fork to the `dev` branch in `https://github.com/am-0code1/cv`.


