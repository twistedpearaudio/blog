---
title: Cronus Platform Overview
layout: page
---

## Introduction
---
The Cronus platform is a system of integrated modular components that, when used together, make up an ideal, low jitter source by ensuring that all audio data is precisely aligned with the master clock used by the source itself. 
There are two related functions that make this possible: isolation and reclocking. Either alone would not be enough because each treats a different mechanism for sources of phase noise.  We designed the Cronus system to provide an ideal digital audio source for a wide variety of audio DACs.  When in use, the Cronus system becomes the actual source standing in as a proxy for the original source.

* TOC
{:toc}

## Key Terms
---
- **Source:** The hardware source of the audio stream.  For example, Amanero USB module, BeagleBone Black SoC, Raspberry Pi, etc. The Cronus platform currently supports only sources that can accept an external master clock or bit clock.
- **DAC:** The audio digital to analog converter such as the Buffalo III/III-SE or any other audio DAC capable of accepting the audio format delivered by the Source.
- **Digital Audio Stream:** The Cronus platform itself does not dictate any particular audio format - but does assume that the clocks and data channels of the audio stream can and should be time aligned with the master clock.  Examples of supported audio streams are PCM(LJ, RJ, I2S), SPDIF, and DSD; the Cronus is designed and tested to work any of these.
- **Phase Noise:** In signal processing, phase noise is the frequency domain representation of rapid, short-term, random fluctuations in the phase of a waveform, caused by time domain instabilities ("jitter").

## The Modules Types
---
### The Cronus Module
The Cronus module itself is the “master” module, providing a precise, clean, and accurate master clock to the source, as well as reclocking the audio stream signals and passing them along to the audio DAC. It is the master interface between a Hermes module (see below) and the DAC. Cronus provides all of the critical functions as it relates to the final signal. 

Please note that any single audio signal frequency from the Hermes header must be at most half the master clock frequency.  For example, if the source can supply a bit clock for a 384Khz PCM signal at 64fs which is 24.576 Mhz - you will need a master clock at double that frequency. We supply Rhea clock modules for the most common use cases.
### Components:

- On-board ultra-low noise and low output impedance voltage regulator for supplying the clocks and the critical signal handling devices on the module. This regulator is placed close to the clocks for maximum performance.
- 2:1 Clock Multiplexer (MUX) for selecting between audio frequency families - almost always 44.1Khz and 48Khz multiples. The MUX is switched via the CS (Clock Select) signal from the source via the Hermes header. 
- An ultra-low noise 1:1, 1:2, 1:4 clock divider for supplying the source with a clock that is suitable for creating a stream.  This allows use of a master clock that is faster than the actual source can accept.  For instance, streaming a 384Khz I2S signal with a 64fs bit clock requires the Cronus have a master clock frequency of 49.152 Mhz, but the BeagleBone Black is specified to be sourced by a maximum audio clock  frequency of 25Mhz.  The master clock is therefore divided by two by selecting the 1:2 divider output to route to the Hermes-BBB.  Specific clock requirement details will be specified in the sections for each specific Hermes module. 
- A synchronous reclocking stage based on Potato Semiconductor CMOS logic with noise cancellation technology for aligning the signals to the master clock just prior to sending them along to the DAC without errors. This is the critical stage  for producing a noise free audio source.
- DIP sockets for DIP8/14 style clock modules, such as the Rhea.  3.3VDC is supplied by the on-board regulator. Clocks used in these sockets should be CMOS output types.
- An ultra low phase noise clock buffer to isolate the clocks from loads. 

---

## The Hermes Modules

Hermes modules act as an isolation interface between a source (Amanero, BBB, Raspberry Pi, etc) and the Cronus module. They carry a master clock signal to the source and a digital audio stream and clock select (C/S) back to the Cronus. 

Hermes modules can sometimes add additional features (mostly optional) specific to the target source.  For example, the Hermes-BBB module includes features specific to the BeagleBone Black, such as exposing I/O, battery/power features, a prototyping area, etc.  The use of these features is not directly related to the main function of the Hermes/Cronus pair, but allow better utilization of the many features the source has to offer.

A key benefit of the Hermes design is that we can utilize new sources without needing to redesign or replace the whole system; we only need create a Hermes module for the new source.  If you already have a Cronus module and suitable clocks/Rheas and wish to try a new source, you need only obtain the appropriate Hermes module for the source. 

### Core Components:
- Header to interface with the Source matching the source pin-out.
- Header to interface with the Cronus matching the Cronus/Hermes pin out.
- A high-speed digital isolator powered on the input side by the source (typically) and on the output (clean) side by the Cronus’ ultra-low noise voltage regulator via the Hermes output header.

**Importantly**, no other power is required on the Cronus side for any current Hermes module, though some modules such as the Hermes-BBB have power headers specific to that source; they are used only by the source itself, and are not connected to the Cronus, nor the “clean” side of the Hermes. See details on individual Hermes modules for more details.

## Rhea Modules
Rhea modules are simple but important (they are after all the master clocks from which all signals will be derived) 
pluggable/replaceable clock modules. They are designed to fit in the DIP sockets provided on the Cronus. 
This module makes it easy to use several types CMOS level clocks (XOs or Crystal Oscillators) and we supply excellent 
examples of the most commonly required master clock frequencies. The C/S(Clock Select) signal coming in from the Hermes 
