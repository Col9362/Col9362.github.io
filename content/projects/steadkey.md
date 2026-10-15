---
title: "SteadKey: Parkinson's Accessible Keyboard"
date: 2026-03-13
summary: "A keyboard and trackball for people with tremors, shaped by interviews with patients and neurologists. Built on a custom key-matrix PCB with RGB layer indicators."
tech: ["Arduino Nano ESP32", "KiCad", "Onshape", "C++", "PCB Design"]
image: "images/projects/steadkey/final.jpg"
weight: 2
---

## Overview

SteadKey is a keyboard for people with Parkinson's disease and essential tremor. Many
of them find standard keyboards and mice so frustrating that they stop using computers
altogether. Our team of four built it over three generations from May 2025 to March
2026. I was the **lead programmer, CAD designer and PCB designer**.

The final version, shown above, has large keys. Several characters share each key, and
you switch between them in layers using shift keys or two foot pedals. RGB LEDs show
which layer you're on. It also has a captive trackball in place of a mouse and a small
OLED screen that suggests the next word as you type.

## Designing with users, not just for them

We built the design around interviews with people living with Parkinson's and with
neurologists. Here's what they told us and how we changed SteadKey in response.

**People with Parkinson's.** The person I interviewed, a family friend with Parkinson's,
told me it was so hard to avoid mistakes that they had become afraid of using computers
at all. With no real workaround, they relied only on voice-to-text on their phone. When I
asked what would help, they named big buttons, a mouse that stays still (a trackball) and
autocomplete. Another interviewee said they avoided keyboards completely and had other
people type for them, and that bigger keys would cut down their constant missed
keystrokes. Those conversations set the core feature list: oversized keys, a built-in
trackball and word prediction.

**Neurologists.** We interviewed two neurologists, who gave us complementary advice:

- One explained that tremor types differ. Parkinson's tremors are often worst when the
  hand is at rest, while other people shake mostly while moving. He told us to design
  for both. He also endorsed the trackball, since a regular mouse is easily thrown off by
  tremors. He suggested a **wrist rest** and suggested basing the layer system on **old
  cellphone keypads**, so it would feel familiar.
- The other gave a harder message: switching layers can be **too complicated**,
  especially for patients with cognitive decline. That warning pushed us to make layer
  switching as easy as possible.

**How that feedback shows up in the final design:**

- **Two ways to switch layers.** Users can press dedicated left and right shift keys on
  the keyboard or use the foot pedals, so people who struggle with one method can use
  the other. The RGB backlight makes the current layer obvious at a glance.
- **A letter layout based on letter frequency**, arranged so most words need no more
  than two layer shifts.
- **A wrist rest and an angled case** to steady the hands.
- **A trackball that stays captive.** It can't fall out when the device is bumped or
  carried around.
- **A tremor delay setting** that filters out accidental keypresses, plus adjustable
  trackball speed and LED brightness, because every user's tremor is different.
- **Printed keycap labels.** Our hand-drawn labels were messy and hard to read, so we
  replaced them with printed sticker labels.

## Electrical design

I led the PCB design for SteadKey in KiCad. Each generation fixed problems found in
the one before.

### Generation 1: proof of concept

Generation 1 had a small board with a USB-C microcontroller, nine multi-letter keys,
discrete LEDs to show the layer, and connectors for the two foot pedals. Each key used
its own input pin, which capped the number of keys we could have.

### Generation 2: key matrix, RGB LEDs and the trackball

![Generation 2 PCB layout, shown in the JLCPCB Gerber viewer](/images/projects/steadkey/gen2-pcb.jpg)

To move to a full layout, I redesigned the board around a **diode key matrix**. Each
switch sits where one row line crosses one column line, so the microcontroller can scan
dozens of keys with a handful of pins. The diodes stop "ghost" keypresses when several
keys are held down at once.

For layer indication I used **SK6812MINI-E addressable RGB LEDs** under the keys. They
connect in a daisy chain, so one data pin controls all of them. Each LED has its own
100 nF decoupling capacitor to keep the power supply stable when they switch.

The trackball uses a **PMW3360 optical sensor** on a breakout board. The breakout
connects to the main PCB through a 7-pin JST connector and sits under a 3D-printed
mount that spins the ball on 3 mm ceramic bearings. An OLED connects over SPI.

This generation taught me a hard lesson about checking footprints. I'd picked
capacitors far too small to hand-solder, so we had to learn to reflow them onto the
board with a hot-air gun.

![Generation 2 prototype](/images/projects/steadkey/gen2.jpg)

### Generation 3: smaller, cleaner, professionally assembled

![Generation 3 PCB layout in KiCad](/images/projects/steadkey/gen3-pcb.jpg)

For the final version I kept the Generation 2 schematic, fixed a **5 V connection** that
had been missed, and re-laid out the board with the goal of shrinking it:

- I moved the JST connectors for the trackball, OLED and foot pedals to the **bottom
  layer**, which saved more than 30 mm.
- Tighter spacing between key groups saved about another 30 mm. The keys were still
  large enough to stay accessible.
- Moving the microcontroller, an **Arduino Nano ESP32**, saved about 30 mm more.
- I mounted the optical sensor **under** the PCB, so the trackball sits lower and is
  more comfortable to use.

To avoid weeks of hand-soldering tiny parts, I sent the board to JLCPCB with a
pick-and-place position file, so their machines soldered the surface-mount capacitors
and diodes. We then added hot-swap switch sockets and the LEDs by hand.

![Generation 3 PCB, assembled](/images/projects/steadkey/gen3-board.jpg)

### Firmware

I wrote the firmware for the Arduino Nano ESP32 in C++. It uses both of the ESP32's processor
cores: one scans the key matrix and reads the trackball, and the other runs
autocomplete. That way word prediction never makes typing feel laggy. Autocomplete
draws on a word-frequency dictionary stored in the ESP32's flash memory and learns the
user's most-typed words over time. A settings menu on the OLED lets users adjust LED
brightness, tremor delay, trackball speed and auto-capitalization.

## What I learned

The biggest lesson was that the people we built for were better designers than we
were. Features we were excited about, like more layers, turned out to be confusing.
Small details, like a pedal you can rest your foot on or a delay that ignores a shaky
double press, made the biggest difference. On the electrical side, Generation 3 taught
me to design for manufacturing from the start: pick parts that can be assembled, check
every footprint, and use both sides of the board.
