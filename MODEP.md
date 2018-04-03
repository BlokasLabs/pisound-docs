# MODEP - MOD Emulator for Pisound
![modep-intro](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/modep-intro.PNG)

## Introduction

MOD or Musician Operated Device is “a multi-effects pedal that pushes the limit of your guitar, bass, keyboard or any other instrument to craft the sounds you want and reproduce them instantly at home, studio or on stage.”

MOD is based on Linux SBC and LV2 plugins ecosystem. It has an intuitive drag-and-drop web-based interface so you can assemble your pedalboards as you do in real life. You can find more information about MOD workflow [here](https://moddevices.com/pages/mod-duo).

As it should be clear for you by now, MODEP is an emulator that will allow you to play around with MOD system using your Raspberry Pi and Pisound board.

By the way, MOD is not only software, it’s also a nice piece of hardware and the MOD team has done a remarkable job for the whole Linux audio open-source community, so if you like this emulator you should get the real thing @ https://moddevices.com.

## Setup Instructions
![modep-setup](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/modep-setup.png)

The fastest way to start messing around with MOD system is to download already prepared MODEP image file based on Rasbian Lite OS [here](http://www.mediafire.com/file/oya6bq5sct658ba/modep.zip) (follow [these instructions](https://www.raspberrypi.org/documentation/installation/installing-images/) to install the image on your SD card).

For manual setup visit [this page](https://github.com/BlokasLabs/modep).

## Running MODEP
- Mount your freshly baked SD card to your Pi
- Power up your Raspberry Pi
- Connect to it via ‘Pisound' Wi-Fi hotspot using your computer or tablet (psw:blokaslabs)
- Open your browser and go to this address `172.24.1.1` (you should see something like in the image below)
- That’s it. Now you can start building your pedalboards

Note that Jack is running as 'root' user, in case you'd like to manually run jack_capture or other jack utils, it should be done as 'root' too.

![modep-default](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/modep-default.png)

## Configuration

- ssh pi@<Your Raspberry´s IP>          # for builds before 1/1/18 
- ssh modep@<Your Raspberry´s IP>       # for builds after 1/1/18
- Password: blokaslabs
- type `modep` press enter.

## The Button

Here is the list of functions you can achieve using Pisound’s button.

|**Interaction**|**Action**|
|:-----|:-----|
| Click 1 to 8 times | to load the pedalboard from the first bank on your list at index corresponding to the number of clicks (see the image below).|
| Hold for 1 second  | to turn Wi-Fi hotspot mode on/off.|                                   
| Hold for 5 seconds | to turn your Raspberry Pi off.|                                                                           

![modep-banks](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/modep-banks.PNG)

## Quick and Dirty Debug
### Option 1 - How to find your IPv4-Address
- If your pi is connected to your local network via cable:
- Login into your local router via its webgui
- Go to network or network overview.
- Look up a device called pisound
- the IP should be listed somewhere there.

### Option 2 - How to find your IPv4 Address
- Connect Screen and Keyboard to your PI
- You should see a text-based terminal (the linux cli)
- Login in with user "pi" or "modep" user-account
- type ifconfig -a to show all network interface configurations
- if connected by wlan-hotspot look up the entry for "wlan0" field "inet"
- if connected via ethernet (cable) look up the entry for "eth0" field "inet"
- this is the IP-Address of your device

### Test connectivity
- test connectivity to the raspberry from your pc/laptop (e.g. windows) by
- running "cmd" and enter "ping <IP-Address>" 
- if connectivity is good, no error should return.

### Web-GUI is reachable, but no sound or "add device error"
- Check input, output cabling, volume and gain level. If this fails:
- login via ssh (e.g. by using putty for windows)
- start configuration script "modep".
- select "3 Tools" (Arrow-Keys and TAB)
- select "4 Disable MODEP systemd services"
- press "enter", wait til finished and pressed "enter" again.
- select "3 Enable MODEP systemd services"
- press "enter", wait til finished and press "enter" again.
- select "1 Check system status" - All services should state "active"
- browser should reload automatically, try again.

### Disturbed Sound
- PISOUND´s input is hard and harsh clipping
- First check if "clipping" lamp is flashing on the PISOUND while audio input is provided to the PI.
- Check "GAIN"-Level (turn it down to minimum, not middle position)
- Reduce audio input-level by turning down the volume of the connected input device.

## Plugins

At the moment MODEP image comes with 144 LV2 plugins ranging from synths to heavy guitar distortion plugins.

