---
description: Pisound's physical properties, power supply recommendations, supported Raspberry Pi models, and pinout details.
---

# General Pisound Specifications

Pisound conforms to Raspberry Pi’s Hardware Attached on Top (HAT) specifications and connects to Raspberry Pi via the 40-pin header. The shield is slightly bigger in length (56 mm x 100 mm) than Raspberry Pi itself. It has two female DIN-5 connectors for MIDI in/out and two 1/4" (6.35 mm) stereo jack connectors for stereo audio in/out. There are two pots for gain and volume control, a programmable button and MIDI activity and input clip LEDs.

**Parameter**|**Value**
:-----|:-----
Current Draw|< 300 mA @ 5.1 VDC
Dimensions|56 mm x 100 mm
Weight|67 g


## Power Supply 

Pisound powers up from Raspberry Pi's power supply via pins on the GPIO header and consumes no more than 300 mA at 5.1 VDC, so we recommend to use the official <a href="https://www.raspberrypi.org/products/raspberry-pi-universal-power-supply/" target="_blank">5.1 VDC RPi power supply for RPi version 1, 2 and 3</a> or <a href="https://www.raspberrypi.org/products/type-c-power-supply//" target="_blank">USB-C Power Supply for RPi version 4</a>.


## Supported Raspberry Pi Models

| **Compatible Models** |  |
| ----- | ----- |
| Raspberry Pi 4[^1] | Raspberry Pi 3 |
| Raspberry Pi 3B+ | Raspberry Pi 2 version 1.2 |
| Raspberry Pi 2 | Raspberry Pi 1 Model B+ |
| Raspberry Pi 1 Model A+ | Raspberry Pi Zero version 1.2|
| Raspberry Pi Zero version 1.3| |

[^1]:
    Full compatibility since Pisound hardware version v1.1.

    Pisound v1.0 can still be used with the RPi model 4, but there's a known issue caused by a power supply design change in Raspbery Pi 4, see
    [this topic](https://community.blokas.io/t/pisound-with-raspberry-pi-4/1238/12?u=giedrius) in our community for detailed information and
    software workaround details. [`pisound-config`](pisound-config.md) has a menu to help with enabling / disabling it.

///Footnotes Go Here///

## Raspberry Pi Pins Used by Pisound
![pinout map rev3](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/pisound-pins.png)

* Black - Power supply pins.
* Red - Pins used by Pisound.
* Green - Pins available for your use.
* Blue - Pins reserved for Raspberry Pi hats use.

## Pinout of Pisound Header
 
![pisound-header](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/pisound-header.png)

**Number**|**Corresponding Raspberry Pi Pin**
:-----|:-----
1|Ground
2|5 V Power
3|BCM 7 (CE1)
4|3v3 Power
5|BCM 5
6|BCM 6
7|BCM 22
8|BCM 23
9|BCM 27
10|BCM 4 (GPCLK0)
11|BCM 15 (RXD)
12|BCM 14 (TXD)
13|BCM 2 (SDA)
14|BCM 3 (SLC)
