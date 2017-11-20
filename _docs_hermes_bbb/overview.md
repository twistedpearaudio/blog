---
title: Hermes BeagleBone Black - Overview
layout: page
---

# Hermes Beagle Bone Black

[Schematic]({{ site.baseurl }}{% link _docs_hermes_bbb/assets/hermes-bbb-schematic.pdf %})

## Here are some of the features:

-    Designed specifically to work with the Cronus - but can work with any other selectable clock source.
-    Exposes an isolated clock select output and master clock input.
-    Fully isolated audio signal with 1 -8 channels of PCM - 1 - 4 channels of DSD output. All I/O to the Cronus and/or DAC is fully isolated.
-    SPDIF output is possible.
-    Header for Switches/Rotary encoder
-    Header and drivers for indicator LEDs. These can be re-purposed for GPIO via the "FLAGS" header.
-    Auto selection of DSD vs PCM routing for DACs that multiplex the LRCK signal with a DSD channel. This means it works well with the BIII-B3SE etc.
-    Fully isolated and non-isolated access to I2C for interacting with DACs and other devices.
-    Jumpers for either powering the clean side of the I2C isolator from the Cronus or the DAC.
-    USART header
-    ADC header for analog control.
-    Headers for external power/reset switches.
-    Prototyping area (for fun!)
-    Provision for backup battery to protect the BBB on shutdown by providing a soft shutdown(self regulating battery not included in the kit - but very easy/cheap to obtain).


This module is one of a few Hermes module we have designed to connect directly to a Cronus module which provides the master clock and relocks the audio data to remove any phase noise.

It has been thoroughly tested and works with any audio signal which the BBB can generate. I have tested up to 384Khz 32-bit PCM. Similar data rates for DSD would be no problem.

The isolators can handle input and output in excess of 100Mhz without any issues.

It is designed to mate directly with with BBB and the Cronus for excellent signal integrity and no wiring mess.

Battery Selection

The Hermes comes with a 10K resistor across the NTC terminals for batteries that are self protected against overcharging/current/discharging - LiPo type "062535" (search ebay) are suggested. 250-500ma is more than adequate. This is the only type of battery we recommend!!!

It is possible to use other battery types - but it is not suggested - because the battery manager on the BBB does not protect from undervoltage nor accidental shorts (over current).

