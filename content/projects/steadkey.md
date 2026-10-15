---
title: "SteadKey: Parkinson's Accessible Keyboard"
date: 2026-03-13
summary: "An accessible keyboard for people with tremors, with large keys, layers shown by RGB LEDs, a captive trackball, foot pedals and an autocomplete module."
tech: ["Arduino", "KiCad", "Onshape", "Python", "PCB Design"]
image: "images/projects/steadkey/final.jpg"
weight: 2
---

## Overview

SteadKey is a keyboard for people with Parkinson's disease and essential tremor,
who often find standard keyboards and mice hard to use. Our team of four built it over
three generations from May 2025 to March 2026. I was the **lead programmer, CAD
designer and PCB designer**.

The final version has a compact 75% layout with large keys. Several characters share
each key, and you switch between layers to reach them, with RGB LEDs showing which
layer is active. It also has a captive 44 mm trackball that replaces the mouse, foot
pedals, and an OLED autocomplete module that suggests the next word.

![SteadKey Generation 3 close-up](/images/projects/steadkey/closeup.jpg)

## Research

We surveyed 41 people with Parkinson's and their caregivers. **82.9%** said improving
how they use electronic devices was important or very important. Respondents rated
current keyboards and mice only 2 to 2.5 out of 5 for ease of use. They rated larger
keys (4.2), easy-to-press keys (4.3), a smaller keyboard (3.8) and a trackball (3.4)
much higher, and those ratings shaped the final design.

## What I did

- Designed the custom PCBs in KiCad, including a redesign for Generation 3 that put
  the connectors on the bottom layer to make the board much smaller
- Designed the trackball and casing in Onshape, and iterated the trackball until it
  stayed captive through 60 seconds of shaking
- Programmed the keyboard firmware: layer switching, RGB LED layer indicators,
  trackball input and the autocomplete module
- Optimized letter and key placement with Python to reduce hand travel and the number
  of layer shifts per word

![Generation 2 prototype](/images/projects/steadkey/gen2.jpg)

## Results

![Generation 3 test results](/images/projects/steadkey/testing.jpg)

- **LED layer indication:** 96 to 100% accurate across five trials of 60 rapid layer
  switches each, with no accidental double presses
- **Autocomplete:** four users each wrote a standard-length email with and without
  autocomplete, and were about **one minute faster** on average with it
- **Trackball:** stayed secure in every shake trial

## What I learned

Building three generations taught me how much testing with real users changes a
design. Things that looked fine on the bench, like keycaps labeled by hand or a
trackball that could fall out, were the first problems users ran into.
