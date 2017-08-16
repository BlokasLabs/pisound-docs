# Getting Started
![pisound-side](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/pisound-front.jpg)
![pisound-side](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/pisound-back.jpg)

Pisound is an ultra-low latency high-quality soundcard and MIDI interface specially designed for Raspberry Pi pocket computers. Equipped with 192kHz 24-bit [Stereo Input and Output](audio) driven by the legendary Burr-Brown chips, DIN-5 [MIDI Input and Output](midi) ports, [user-customizable button](the-button) and bundled [software tools](software), it has everything you need to bring your audio projects to life in no time.

## Hardware Setup

### Pisound
You should find 1 Pisound and a plastic bag containing 2 knobs, 8 screws and 4 spacers in the Pisound box.

Mount Pisound on top of your Raspberry Pi via the 40-pin header and fasten it with the screws provided while the RPi is unpowered, so it appears as in the image at the top. See compatible Raspberry Pi models [here](specs#supported-raspberry-pi-models).

### Pisound Acrylic Case
The Pisound Acrylic Case consists of 2 cutout sheets of the enclosure sides and a plastic bag containing 4 short spacers, 4 longer ones and a button cap.

## Driver Setup

If you don't have any Linux OS running on your Raspberry Pi, we suggest starting with [Raspbian](https://www.raspberrypi.org/documentation/installation/installing-images/README.md).

[Power your Raspberry Pi up](specs#power-supply) and install the driver by running the below commands in a terminal window:

```bash
wget http://blokas.io/pisound/install-pisound.sh -O install-pisound.sh
chmod +x install-pisound.sh
./install-pisound.sh
```

A couple of simple [Y/n] questions will be presented about whether to install some of the additional software to get you started faster and whether to set Pisound as the default card.

After driver installation process is complete, reboot your Raspberry Pi (if the script asked for it at the end) and that's it. Now you can choose Pisound as your main sound card through native Volume widget and use it with any Linux Audio/MIDI software. Done! Thank You!

If you hit any issues during driver setup, please see [Verifying It Works](software#verifying-it-works) and provide us [feedback](software#feedback).

## Connect Things

![pisound-connections](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/connections.png)

Raspberry Pi in combination with Pisound allows you to connect a huge variety of different types of audio-related gear. Thanks to Pisound's Stereo Input with wide-range gain control design, you can connect audio sources ranging from your bass guitar to a CD player or your modular synth. Using Stereo Output you can hook Pisound to any mixer, speakers or just plug in your headphones. Volume and Gain levels can be controlled using on-board knobs. For MIDI connectivity you also have a lot of options - MIDI In/Out through on-board DIN-5 sockets, USB-MIDI via RPi's USB ports or even [WiFi-MIDI](the-button#toggle-wifi-hotspot-mode)!

## Print Your Own Case

To take the setup process one step further, you can even 3D-print your own case. All necessary files can be found [here](https://github.com/BlokasLabs/pisound-case).

![pisound-case](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/pisound-case.png)