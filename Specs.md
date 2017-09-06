# Specifications

The shield itself conforms to Raspberry Pi’s Hardware Attached on Top (HAT) specifications and connects to Pi via the 40-pin header. The shield is slightly bigger in length (56x100 mm) than RPi itself. It has two female DIN-5 connectors for MIDI in/out and two 1/4" (6.35mm) stereo jack connectors for stereo audio in/out. There are two pots for gain and volume control, a programmable button and MIDI activity and input clip LEDs.

## Audio

**Parameter**|**Conditions**|**Value**
:-----|:-----|:-----
Input/Output Coupling|-|AC and DC respectively
Input/Output Channels|-|2 / 2 (Left and Right)
Input/Output Type|-|¼" (6.35mm) Stereo
Input/Output Resolution|-|24bit
Sampling Frequency (Fs)|-|48kHz, 96kHz, 192kHz
Input/Output SNR@1kHz|G = 0 dB|110dB
Input Impedance|-|100kOhm &#124;&#124; 2pF
Input Gain (G)|-|0dB to +40dB
Input Clip LED|-|Yes
Input Clip Voltage|G = 0 dB|5V (peak to peak)
Full Scale Output|Load impedance > 1 kOhm|0V to 2.1V (RMS)
Loopback Bandwidth (-3 dB)|G = 0 dB, Fs = 48 kHz|7.5Hz - 23kHz
Loopback THD@1kHz|G = 0 dB, Fs = 48 kHz|< 0.045%
Loopback Latency|Fs = 192 kHz, RPi2, buffer size = 128 frames|2.092ms
Phantom Power|-|None

## MIDI

**Parameter**|**Value**
:-----|:-----
Input/Output connectors|DIN-5 female sockets
MIDI loopback latency|2.105ms
Activity LEDs|Input & Output

## Other

**Parameter**|**Value**
:-----|:-----
Current Draw|< 300mA @ 5.1VDC
Dimensions|56mm x 100mm
Weight|67g


## Power Supply 
There are two versions of Pisound regarding power supply:

* **9V version (beta):** 7.2V - 12.6V, 18W minimum. 5.5x2.1 mm coaxial power jack connector. The inner connector is connected to the positive terminal, and the sleeve is connected to the ground. The power adapter connected to Pisound supplies the RPi board too, so RPi does not need to have its USB supply port connected. The Pisound itself has a power consumption of about 1.8W. A 9VDC power supply capable of delivering at least 2 Amps of current is recommended for this version.

* **5.1V version (latest):** Pisound has no power connection and requires no additional power supply. It powers up from RPi power supply via pins on RPi header. Pisound consumes no more than 300mA at 5.1VDC. When using this version of Pisound, we recommend to use the official [5.1VDC RPi power supply](https://www.raspberrypi.org/products/universal-power-supply/).

## Supported Raspberry Pi Models

**Compatible models**|
:-----|
Raspberry Pi 1 Model A+|
Raspberry Pi 1 Model B+|
Raspberry Pi 2|
Raspberry Pi 2 version 1.2|
Raspberry Pi 3|
Raspberry Pi Zero version 1.2|
Raspberry Pi Zero version 1.3|


## Raspberry Pi pins used by Pisound
![pinout map rev3](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/pisound-pins.png)

* Black - Power supply pins.
* Red - Pins used by Pisound.
* Green - Pins available for your use.
* Blue - Pins reserved for Raspberry Pi hats use.

## Pinout of Pisound Header
![pisound-header](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/pisound-header.png)