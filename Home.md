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