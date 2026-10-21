---
title: "SteadKey: Parkinson's Accessible Keyboard"
date: 2026-03-13
summary: "A keyboard and trackball for people with tremors. We built three versions over a school year, working off interviews with patients and a neurologist."
tech: ["Arduino Nano ESP32", "KiCad", "Onshape", "C++", "Python", "PCB Design"]
image: "images/projects/steadkey/final.jpg"
weight: 2
---

## Overview

SteadKey is a keyboard made for people with Parkinson's and essential tremor. A lot of
people with tremors have a really hard time with normal keyboards and mice because fine motor impairments make it difficult to move their hands precisely. Some just stop using computers altogether. It was a four-person project in my engineering class, and we went through three versions between May 2025 and March 2026. I did the programming, the PCB design and most of the CAD.

The keys are big, 1.5x the standard key size in both length and width, and each one holds 3 characters. You switch between layers with the left and right layer shift keys or with two foot pedals, and RGB LEDs under the keys change color so you know which layer you're on (red, green, blue). There's a trackball built in so you don't need a mouse, and a small OLED screen that shows autocomplete suggestions. The keyboard connects to a computer via the Arduino Nano ESP32's USB-C port. 

## Talking to users

Before designing the second version, we talked to people with Parkinson's and to a
neurologist.

The person I interviewed is a family friend with Parkinson's. They said that since it's hard
not to mess things up on a computer, they'd become afraid to use one and only
use voice-to-text on their phone. When I asked what would help, they said big buttons,
a mouse that doesn't move (basically a trackball) and autocomplete. Another person we
talked to said they avoid keyboards and have other people type for them, and that
bigger keys would help with all the keys they miss.

The neurologist also added a lot. He explained that Parkinson's tremors are usually worst
when the hand is resting, while essential tremor is worse when the hands are in use, so the keyboard
should work for both. He liked the trackball idea since a mouse is hard to control with
a tremor. He also suggested a wrist rest, and basing the layers on old cellphone keypads
so they'd feel familiar. However, he did warn us that switching layers could be too confusing for patients with some cognitive decline.

A lot of the final design came out of those conversations:

- You can switch layers with the layer shift keys or the foot pedals, whichever is easier
  for you, and the LEDs make the current layer easy to see.
- The letters are arranged by how often they're used, so most words take two layer
  shifts or fewer.
- There's a wrist rest and the case is angled.
- The trackball is held in so it can't fall out if the keyboard gets bumped or moved.
- A "tremor delay" setting ignores accidental extra presses, and trackball speed and
  LED brightness are adjustable too.
- An OLED acts as an autocomplete module, displaying the 5 most likely words being typed as well as predicting the next word typed after registering punctuation or a space press.
- We replaced our hand-drawn key labels with printed stickers because the hand-drawn
  ones were hard to read.

## Letter layout

Rather than having the keys be alphabetical like a flip phone (A, B, C on one key, D, E, F on the next, etc.), we used Python to analyze the most common bigrams and trigrams (combinations of 2 and 3 letters) in order to see which letters are commonly used together. This was done to optimize the layout and reduce the amount of layer switching needed to operate our device. The tradeoff with this was that users no longer had the familiarity aspect, but it proved to not be a big issue.

## Electrical design

I led the PCB design in KiCad. Each version fixed problems from the last one.

### Generation 1

The first board was small: a USB-C microcontroller, nine keys with several letters each,
a few LEDs to show the layer, and connectors for the foot pedals. Every key had its own
input pin, which limited how many keys we could fit. We also had an up/down and left/right mouse movement key to prove that we could control a computer's mouse input via our device, which was a success.

### Generation 2

To get to a full layout I switched to a key matrix with diodes. Each switch connects
one row and one column, so the microcontroller can read a lot of keys with only a few
pins, and the diodes stop false presses when multiple keys are held down, giving us n-key rollover.

For the layer lights I used SK6812MINI-E addressable RGB LEDs. They're chained together,
so one data pin controls all of them, and each one has a 100 nF decoupling capacitor.

The trackball uses a PMW3360 optical sensor on a small breakout board that plugs into
the main PCB with a 7-pin JST connector. The ball sits on 3 mm ceramic bearings in a
3D-printed mount. The OLED connects over SPI.

I made one big mistake on this board: I picked capacitors that were way too small to
solder by hand. We ended up learning to reflow them with a hot-air gun.

![Generation 2 prototype](/images/projects/steadkey/gen2.jpg)

### Generation 3

![Generation 3 PCB layout in KiCad](/images/projects/steadkey/gen3-pcb.jpg)

For the final board I kept the Generation 2 schematic, fixed a 5 V connection we'd
missed, and redid the layout to make it smaller. Moving the JST connectors for the
trackball, OLED and pedals to the bottom of the board saved over 30 mm. Tightening the
spacing between key groups saved about 30 mm more, and moving the microcontroller (an
Arduino Nano ESP32) saved another 30 mm or so. I also moved the trackball sensor to the
underside of the board so the ball could sit lower.

This time I had JLCPCB solder the tiny capacitors and diodes for us, which saved a lot
of time. We soldered the hot-swap sockets and LEDs ourselves.

![Generation 3 PCB, assembled](/images/projects/steadkey/gen3-board-pcb.jpg)

## Firmware

I wrote the firmware in C++ for the Arduino Nano ESP32 using the Arduino IDE. The ESP32 has two cores, so one
handles the keys and trackball and the other runs autocomplete, which keeps typing from
lagging. Autocomplete uses a word-frequency list stored in flash and picks up the words
you type most. There's also a settings menu on the OLED for LED brightness, tremor
delay, trackball speed and auto-capitalization.

## What I learned

Talking to actual users changed the project more than anything else. Ultimately, you have to design for the user, not for what you think is cool. Some features we
were excited about, like adding more layers, would have just made it confusing, and
small things like a pedal you can rest your foot on mattered a lot more than we
expected. On the hardware side, I learned to think about how a board will actually get
built: check every footprint, pick parts you can really solder, and use both sides of
the board.
