---
title: "Modular Handheld Device"
date: 2026-06-01
summary: "A handheld with swappable control modules that attach magnetically. The base figures out which module is plugged in and loads the right controls."
tech: ["MicroPython", "Raspberry Pi Pico", "KiCad", "Onshape", "I2C"]
image: "images/projects/handheld/assembled.jpg"
thumbnail: "images/projects/handheld/modules.jpg"
github: "https://github.com/Col9362/Handheld_Modular_Device"
weight: 1
---

## Overview

I wanted to see how far I could push one device by making it modular. The base unit
has the screen and the microcontroller, and different control modules snap onto the
bottom. You can swap them while it's on. The first module I built is a gaming
controller, and I'm planning a music module and a keyboard module next.

![The base unit and gaming module, separated](/images/projects/handheld/modules.jpg)

## How it works

The base runs on a Raspberry Pi Pico with MicroPython. It drives an ST7789 touchscreen
over SPI through an 18-pin FPC ribbon cable and has two buzzers for sound. Modules
connect through an 8-pin right-angle magnetic pogo connector on the edge of the board,
which carries power, I2C, an interrupt line, a module ID line and two extra pins for
future modules.

To figure out which module is attached, each module has a resistor on the ID line that
makes a voltage divider with a 1 kΩ resistor on the base. When you plug in or pull out
a module, an interrupt fires and the Pico reads the voltage on that line. The gaming
module reads about 2.2 V. Then it loads the right drivers. If you pull a module off in
the middle of a game, it just goes back to the home screen.

The gaming module has an MCP23017 I/O expander for the seven buttons and the joystick
click, an ADS1115 ADC for the analog joystick, and a DRV2605 haptic driver. All three
talk to the base over the same I2C bus.

![3D render of the base unit PCB](/images/projects/handheld/render-board.png)

![The magnetic pogo connector on the board edge and the display ribbon cable](/images/projects/handheld/render-connectors.png)

## Software

I wrote all the software in MicroPython. It has a home screen and game menu you
control with the joystick and buttons, Tetris (with music and saved high scores), a
settings menu for brightness and volume, and a battery indicator.

## PCB design

The base unit is a two-layer board I designed in KiCad. The Pico is in the middle, the
display connector is on the left, and the pogo connector runs along the bottom edge
where the modules attach.

![Base unit PCB layout: front copper in red, back copper in blue](/images/projects/handheld/pcb-layout.svg)

![Base unit schematic](/images/projects/handheld/schematic-base.svg)

![Gaming module schematic](/images/projects/handheld/schematic-gaming.jpg)

## What's next

I'm planning a music-creation module and a keyboard module.
