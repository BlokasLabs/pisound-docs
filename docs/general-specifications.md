---
description: Pisound's physical properties, power supply recommendations, supported Raspberry Pi models, and pinout details.
---

# General Pisound Specifications

Pisound conforms to Raspberry Pi’s Hardware Attached on Top (HAT) specifications and connects to Raspberry Pi via the 40-pin header. The shield is slightly bigger in length (56 mm x 100 mm) than Raspberry Pi itself. It has two female DIN-5 connectors for MIDI in/out and two 1/4" (6.35 mm) stereo jack connectors for stereo audio in/out. There are two pots for gain and volume control, a programmable button and two MIDI activity and input clip LEDs.

**Parameter**|**Value**
:-----|:-----
Current Draw|< 300 mA @ 5.1 VDC
Dimensions|56 mm x 100 mm
Weight|67 g


## Power Supply 

Pisound powers up from Raspberry Pi's power supply via pins on the GPIO header and consumes no more than 300 mA at 5.1 VDC. We recommend to use one of the official power supplies:

* <a href="https://www.raspberrypi.com/products/27w-power-supply/" target="_blank">USB-C Power Supply for Raspberry Pi version 5</a>
* <a href="https://www.raspberrypi.org/products/type-c-power-supply/" target="_blank">USB-C Power Supply for Raspberry Pi version 4</a>
* <a href="https://www.raspberrypi.org/products/raspberry-pi-universal-power-supply/" target="_blank">Micro USB Power Supply for Raspberry Pi version 1, 2 and 3</a>

## Supported Raspberry Pi Models

| **Compatible Models** |  |
| ----- | ----- |
| Raspberry Pi 5[^1] | Raspberry Pi 4B[^2] |
| Raspberry Pi 3B | Raspberry Pi 3B+ |
| Other Models[^3] | |

[^1]:
    Raspberry Pi 5 Active Cooler may fit in between the Pisound and Raspberry Pi 5 since Pisound hardware version v1.2.

[^2]:
    Full compatibility since Pisound hardware version v1.1.

    Pisound v1.0 can still be used with the Raspberry Pi 4, but there's a known issue caused by a power supply design change in Raspbery Pi 4, see
    [this topic](https://community.blokas.io/t/pisound-with-raspberry-pi-4/1238/12?u=giedrius) in our community for detailed information and
    software workaround details. [`pisound-config`](pisound-config.md) has a menu to help with enabling / disabling it.

[^3]:
    Not recommended for new audio based projects due to lesser processing power, but Pisound can also be used with Raspberry Pi's 2, 1, Zero, Zero W and Zero 2 W as well as the A models.

///Footnotes Go Here///

## Raspberry Pi Pins Used by Pisound
![pinout map rev3](https://raw.githubusercontent.com/BlokasLabs/pisound-docs/master/docs/images/pisound-pins.png)

* Black - Power supply pins.
* Red - Pins used by Pisound.
* Green - Pins available for your use.
* Blue - Pins reserved for Raspberry Pi hats use.

## Pinout of Pisound Header
 
![pisound-header](https://raw.githubusercontent.com/BlokasLabs/pisound-docs/master/docs/images/pisound-header.png)

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
