---
title: "Modular Handheld Device"
date: 2026-06-01
summary: "A handheld built around hot-swappable control modules that snap on through a pogo-pin connector and are detected automatically over I2C."
tech: ["MicroPython", "Raspberry Pi Pico", "KiCad", "Onshape", "I2C"]
image: "images/projects/handheld/assembled.jpg"
thumbnail: "images/projects/handheld/modules.jpg"
github: "https://github.com/Col9362/Handheld_Modular_Device"
weight: 1
---

## Overview

I started this project to learn how modularity can make one device do many jobs.
The base unit holds the brain and the screen. Control modules attach to it and
can be swapped while it's on. The first module is a retro gaming controller.
Next I plan to build a music-creation module and a keyboard module.

![The base unit and gaming module, separated](/images/projects/handheld/modules.jpg)

## How it works

**Base unit:** a Raspberry Pi Pico running MicroPython drives an ST7789 touchscreen
over SPI and two buzzers for sound. An 8-pin pogo connector on the edge carries
power, I2C, an interrupt line, a module-ID line and two expansion pins to whichever
module is attached.

**Hot-swap detection:** every module has a resistor on the ID line that forms a
voltage divider with a 1 kΩ pull-up on the base. When a module is plugged in or
removed, an interrupt fires, the Pico reads the ID voltage on its ADC (about 2.2 V
for the gaming module) and loads the matching drivers. The interface updates on its
own, so unplugging a module mid-game drops you safely back to the home screen.

**Gaming module:** an MCP23017 I/O expander reads seven buttons and the joystick
click. An ADS1115 ADC reads the analog joystick, and a DRV2605 driver adds haptic
feedback. All three share one I2C bus through the pogo connector.

![3D render of the base unit PCB](/images/projects/handheld/render-3d.png)

## Software

I wrote the whole interface in MicroPython:

- A home screen and game menu you navigate with the joystick and buttons
- **Tetris**, with music, accelerated drop and saved high scores
- A settings menu for brightness and volume, saved to flash
- A battery level and charging indicator

## PCB design

I designed the base unit's two-layer PCB in KiCad. The Pico sits in the center, the
display's flat-flex connector is on the left, and the pogo connector runs along the
bottom edge where modules attach.

![Base unit PCB layout: front copper in red, back copper in blue](/images/projects/handheld/pcb-layout.svg)

![Base unit schematic](/images/projects/handheld/schematic-base.svg)

![Gaming module schematic](/images/projects/handheld/schematic-gaming.jpg)

## What's next

- A music-creation module
- A keyboard module
