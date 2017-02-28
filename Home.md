# Getting Started
![pisound-side](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/pisound-side.png)

pisound is an ultra-low latency high-quality soundcard and MIDI interface specially designed for Raspberry Pi pocket computers. Equipped with 192kHz 24-bit Stereo Input and Output driven by the legendary Burr-Brown chips, DIN-5 MIDI Input and Output ports, user-customizable button and bundled software tools, it has everything you need to bring your audio projects to life in no time.

## Hardware Setup

Mount pisound on top of your Raspberry Pi via the 40-pin header and fasten it with the screws provided while the RPi is unpowered, so it appears as in the image at the top. See compatible Raspberry Pi models [here](#supported-raspberry-pi-models).

## Driver Setup

If you don't have any Linux OS running on your Raspberry Pi, we suggest starting with [Raspbian](https://www.raspberrypi.org/documentation/installation/installing-images/README.md).

[Power your Raspberry Pi up](#power-supply) and install the driver by running the below commands in a terminal window:

```bash
wget http://blokas.io/pisound/install-pisound.sh -O install-pisound.sh
chmod +x install-pisound.sh
./install-pisound.sh
```

A couple of simple [Y/n] questions will be presented about whether to install some of the additional software to get you started faster and whether to set pisound as the default card.

After driver installation process is complete, reboot your Raspberry Pi (if the script asked for it at the end) and that's it. Now you can choose pisound as your main sound card through native Volume widget and use it with any Linux Audio/MIDI software. Done! Thank You!

If you hit any issues during driver setup, please see [Verifying It Works](#verifying-it-works) and provide us [feedback](#feedback).

## Connect Things

![pisound-connections](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/connections.png)

Raspberry Pi in combination with pisound allows you to connect a huge variety of different types of audio-related gear. Thanks to pisound's Stereo Input with wide-range gain control design, you can connect audio sources ranging from your bass guitar to a CD player or your modular synth. Using Stereo Output you can hook pisound to any mixer, speakers or just plug in your headphones. Volume and Gain levels can be controlled using on-board knobs. For MIDI connectivity you also have a lot of options - MIDI In/Out through on-board DIN-5 sockets, USB-MIDI via RPi's USB ports or even [WiFi-MIDI](#toggle-wifi-hotspot-mode)!

## Print Your Own Case

To take the setup process one step further, you can even 3D-print your own case. All necessary files can be found [here](https://github.com/BlokasLabs/pisound-case).

![pisound-case](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/pisound-case.png)



# Specifications

The shield itself conforms to Raspberry Pi’s Hardware Attached on Top (HAT) specifications and connects to Pi via the 40-pin header. The shield is slightly bigger in length (56x100 mm) than RPi itself. It has two female DIN-5 connectors for MIDI in/out and two 1/4" (6.35mm) stereo jack connectors for stereo audio in/out. There are two pots for gain and volume control, a programmable button and MIDI activity and input clip LEDs.

## Audio

**Parameter**|**Conditions**|**Value**
:-----|:-----|:-----
Input/Output connectors|-|1/4" (6.35mm) stereo jacks
Sampling frequency (Fs)|-|48kHz, 96kHz, 192kHz
Input/Output resolution|-|24bit
Input/Output SNR@1kHz|G = 0 dB|110dB
Input impedance|-|100kOhm &#124;&#124; 2pF
Input gain (G)|-|0dB to +40dB
Input clip LED|-|Yes
Input clip voltage|G = 0 dB|5V (peak to peak)
Full scale output|Load impedance > 1 kOhm|0V to 2.1V (RMS)
Loopback bandwidth (-3 dB)|G = 0 dB, Fs = 48 kHz|7.5Hz - 23kHz
Loopback THD@1kHz|G = 0 dB, Fs = 48 kHz|< 0.045%
Loopback latency|Fs = 192 kHz, RPi2, buffer size = 128 frames|2.092ms
Phantom power|-|None

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
There are two versions of pisound regarding power supply:

* **9V version (beta):** 7.2V - 12.6V, 18W minimum. 5.5x2.1 mm coaxial power jack connector. The inner connector is connected to the positive terminal, and the sleeve is connected to the ground. The power adapter connected to pisound supplies the RPi board too, so RPi does not need to have its USB supply port connected. The pisound itself has a power consumption of about 1.8W. A 9VDC power supply capable of delivering at least 2 Amps of current is recommended for this version.

* **5.1V version (latest):** pisound has no power connection and requires no additional power supply. It powers up from RPi power supply via pins on RPi header. pisound consumes no more than 300mA at 5.1VDC. When using this version of pisound, we recommend to use the official [5.1VDC RPi power supply](https://www.raspberrypi.org/products/universal-power-supply/).

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


## Pinout of Pi Header
![pinout map rev3](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/pisound-pins.png)

* Black - Power supply pins.
* Red - Pins used by pisound.
* Green - Pins available for your use.
* Blue - Pins reserved for Raspberry Pi hats use.