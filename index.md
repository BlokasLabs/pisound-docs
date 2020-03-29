# Getting Started

Pisound is an ultra-low latency high-quality soundcard and MIDI interface specially designed for Raspberry Pi pocket computers.
Equipped with 192kHz 24-bit [Stereo Input and Output](audio) driven by the legendary Burr-Brown chips, DIN-5 [MIDI Input and Output](midi) ports,
[user-customizable button](the-button) and bundled [software tools](Software), it has everything you need to bring your audio projects to life in no time.

![pisound-side](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/pisound-front.jpg)

![pisound-side](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/pisound-back.jpg)

## Hardware Setup

### Pisound
You should find 1 Pisound and a plastic bag containing 2 knobs, 8 screws and 4 spacers in the Pisound box.

If you intend to use Pisound with the Acrylic Case, refer to the section below on how to assemble it.
Otherwise, mount Pisound on top of your Raspberry Pi via the 40-pin header and fasten it with the screws
provided while the RPi is unpowered, so it appears as in the image at the top.

See compatible Raspberry Pi models [here](Specs.md#supported-raspberry-pi-models).

### Pisound Acrylic Case
![pisound-case](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/pisound-case.jpg)

A simple but really sturdy and beautiful case for your Pisound and Raspberry Pi. The enclosure is cut from semi-transparent 3mm thick acrylic sheet. Round engravings indicate the Output ports and the Output Volume potentiometer. 

The Pisound Acrylic Case consists of 3 cutout sheets of the enclosure sides and a plastic bag containing 4 short spacers, 4 longer ones and a button cap.
The case comes with 2 variations of the side with all of the connectors - one for use with Raspberry Pi version 4, the other is for the rest of the models. Make sure to use the correct one for your Raspberry Pi.

[@mzero](https://community.blokas.io/u/mzero/summary), a member of [Electric Kitchen](https://electric.kitchen) project has made a great detailed Pisound Case assembly video. You can watch it [here](https://youtu.be/vt8rdc14wNY), or you can find the full assembly instructions [here](Pisound-Acrylic-Case.md#assembly-instructions).

## Driver Setup

### Patchbox OS

![patchbox-os](https://blokas.io/patchbox-os/images/1.png)

If you don't have any Linux OS running on your Raspberry Pi, we suggest starting with [Patchbox](https://blokas.io/patchbox-os/). It has everything preconfigured to get you going. Please refer to its [documentation](https://blokas.io/patchbox-os/docs/)
for getting started steps.

### Raspbian

![pisound-config](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/pisound-config.png)

Alternatively, you may want to go with Raspbian. Follow the steps in its [guide](https://www.raspberrypi.org/documentation/installation/installing-images/README.md) to set up the image on your SD card.
Before ejecting the card from your PC, if you don't have a keyboard and monitor hooked up to your Pi, you may want to create a file named 'ssh' in the SD card so that [remote shell](https://www.raspberrypi.org/documentation/remote-access/ssh/) access to a headless device is enabled.
See [this tutorial](https://community.blokas.io/t/raspberry-pi-remote-control-raspberry-pi-via-ssh/597) for more information.

Once your SD card is ready, place it in the SD card slot on the Raspberry Pi. [Power your Raspberry Pi up](Specs.md#power-supply) and install the Pisound software by running the below command in a terminal window:

```bash
curl https://blokas.io/pisound/install.sh | sh
```

This will set up the Blokas APT server and install all the software packages for Pisound. Then you may run [`sudo pisound-config`](Pisound-Config.md) to further customize your installation.

Done! Thank You!

Now you may want to check out the [Pisound Tutorials](https://community.blokas.io/c/pisound/pisound-tutorials/) category in our Community or go through the rest of the documentation.

To make sure everything went fine, please see [Verifying It Works](Software.md#verifying-it-works) and [reach out to us](Software.md#feedback) in case of any issues.

## Connect Things

![pisound-connections](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/connections.png)

Raspberry Pi in combination with Pisound allows you to connect a huge variety of different types of audio-related gear. Thanks to Pisound's Stereo Input
with wide-range gain control design, you can connect audio sources ranging from your bass guitar to a CD player or your modular synth.
Using Stereo Output you can hook Pisound to any mixer, speakers or just plug in your headphones. Volume and Gain levels can be controlled
using on-board knobs. For MIDI connectivity you also have a lot of options - MIDI In/Out through on-board DIN-5 sockets, USB-MIDI via RPi's
USB ports or even [WiFi-MIDI](The-Button.md#toggle_wifi_hotspotsh-toggle-wifi-hotspot-mode)!

## Print Your Own Case

To take the setup process one step further, you can even 3D-print your own case. All necessary files can be found [here](https://github.com/BlokasLabs/pisound-case).

![pisound-case](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/pisound-case.png)
