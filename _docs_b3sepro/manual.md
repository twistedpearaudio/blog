---
title: Buffalo III SE (Stereo Edition) Pro
layout: page
toc: true
---
# Overview
---
The Buffalo DAC modules have a long lineage that tracks closely with the ES90xx series of DAC chips
going all the way from the original Buffalo based on the ES9008 to this module based on the ES9028/38 chips with new
modules still in development.

For standard stereo firmware please see [Buffalo III SE Pro On-Board Firmware](https://github.com/twistedpearaudio/Buffalo-III-SE-Pro-On-Board-Firmware)

**Contents:**
* TOC
{:toc}

## **Features**:
- on-board ultra low phase noise 100Mhz Crystek clock
- accepts both Serial and SPDIF inputs
- on-board consumer to CMOS SPDIF converter 
- on-board controller and port expander for running firmware
- two 8-position DIP switches for configuring firmware
- header for accessing chip GPIOs and Address and Reset signals
- header for accessing controller and port expander
- headers for AVCC,VCC,VDD, and XO(clock) VDD that support both Trident and AVCC modules
- header for Lock/Auto-mute LEDs (on which LEDs can be soldered directly)
- 3.3V regulator for microcontroller and port expander etc
- header for external I2C control including 3.3V(up to 500ma) from controller regulator
- a precision resistor and a filter cap on GPIO2 for DAC optional auto-level adjustment feature (firmware dependant)

# Assembly Notes
---
**Please read this before you start soldering!!!**

## Digital Input Terminal Blocks
**Important!!!** Because we pre-mount uFL connectors on the PCB if you wish to use the terminal blocks you need to mount the terminal
blocks on the bottom of the PCB - even though the silk is on top.
![Soldering Input](images/IMG_2176.jpg)

## Analog Output Headers
When using output stages like the Mercury, Ivy, Legato that mount directly to the Buffalo - it is also recommended you 
solder the female analog output headers to the bottom of the board and the male (long tail) headers to the output stage.
![Soldering Outputs](images/IMG_2177.jpg)

# Power Requirements:

The VD supply is routed to all of the Trident supply header input pins. It also powers the on-board regulator for the
controller. Be sure you you don't exceed the input voltage the Tridents (or any other regulator) and keep in mind that
the ~1.2V supply has quite a drop and will get hotter as voltage rises. 

It is recommended to use Twisted Pear Audio "Trident SR" regulators (Series Reg) "AVCC SR" modules. The AVCC SR module 
has two separate ultra low noise purpose made series regulators for AVCC_L and AVCC_R - it is designed to sit neatly
over the DAC for an ultra short path to the DAC supply pins. 

**Important!!!** Trident SR and AVCC SR should have an input voltage of 5VDC. It is not recommended to exceed 5VDC on 
the VD input when using those modules. 

In all - this module requires 5 regulators (all voltages DC):

- VDD: ~1.2 - 1.3V ~200ma *Note - nominally this is 1.2V but ESS sent a design notice that it should actually be above 1.25V '
and no more than 1.5V when operating with sample rate above 192Khz or DSD128 with the ES9038*
- DVCC: 3.3V ~10ma
- AVCCL: 3.3V-4V ~100ma
- AVCCR: 3.3V-4V ~100ma
- VDD_XO: 3.3V ~ 25ma

*note: current is stated for 44.1khz input signal and 100Mhz master clock current consumption rises with sample rate

# Module Headers

## GPIO

The GPIO header exposes DVCC and GND from the DAC as well as the following:

- GPIO1-4 controllers can set the GPIO pins for special functions. This module facilitates using GPIO4 for indicating
lock and GPIO1 for indicating AUTOMUTE by including series resistors and connecting those nets to an LED header.
If you plan on using GPIO1 or GPIO4 for some other purpose it is recommended you leave the LEDs off.
- DAC_ADDR sets the DAC's I2C address. Open or Low = 0x90 and High = 0x92.
- DAC_RESET active low DAC reset. The on-board controller is connected to this pin. 
Any controller must properly reset the DAC after power up conditions are met. 
**Important** do not short this pin high or low if you are using the on-board controller.

![GPIO Header1](images/IMG_2178.jpg)
![GPIO Header2](images/gpio.png)

## LED(LOCK/AUTOMUTE)

![GPIO Header2](images/leds.png)
![GPIO Header1](images/IMG_2180.jpg)

