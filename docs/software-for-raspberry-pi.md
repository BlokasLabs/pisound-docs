---
description: Raspberry Pi software setup.
---

# Software for Raspberry Pi

If you do not have any operating system running on your Raspberry Pi, you need to install the OS image first. There are different operating systems you may choose from, such as [Patchbox OS](#patchbox-os), [Raspberry Pi OS](#raspberry-pi-os), or other. In order to run the OS image you picked, it must first be <a href="https://blokas.io/patchbox-os/docs/install-os-to-sd-card/" target="_blank">installed to your SD card</a>.

Pisound is compatible with virtually all Linux distributions and software as it comes with an ALSA audio and MIDI driver integrated into mainline Raspberry Pi Linux kernel (ver. 4.4.27+). 

**Note:** In order to get the most from your Pisound you will need to install [The Pisound Software](the-pisound-software.md) to your Raspberry Pi (except if you are using <a href="https://blokas.io/patchbox-os/" target="_blank">Patchbox OS</a>).

## Compatible OSes

Please add or let us know if you have Pisound working on a distribution that is not on the list yet!

- <a href="https://www.archlinux.org/" target="_blank">Arch Linux</a>
- <a href="https://blokas.io/patchbox-os/" target="_blank">Patchbox OS</a>
- <a href="https://www.raspberrypi.org/downloads/raspberry-pi-os/" target="_blank">Raspberry Pi OS</a>
- <a href="https://ubuntu-mate.org/raspberry-pi/" target="_blank">Ubuntu Mate</a>
- <a href="https://volumio.org/" target="_blank">Volumio</a>

## Patchbox OS

![patchbox-os](https://blokas.io/patchbox-os/images/1.png)

For quickest setup, targeted at audio applications, we recommend using the SD card image of our <a href="https://blokas.io/patchbox-os/" target="_blank">Patchbox OS</a>. The Patchbox OS image is based on Raspberry Pi OS Lite and preconfigured with audio software for smooth user experience. On the first boot, a configuration wizard will run and help you to set the system up, including configuring the Jack backend parameters and connecting to a Wi-Fi network. 

**If you are using the Patchbox OS image, you do not need any other installations to get access to all Pisound features!**

Find the Patchbox OS setup instructions <a href="https://blokas.io/patchbox-os/docs/" target="_blank">here</a>.

## Raspberry Pi OS

![pisound-config](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/pisound-config.png)

Alternatively, you may want to go with <a href="https://www.raspberrypi.org/downloads/raspberry-pi-os/" target="_blank">Raspberry Pi OS</a> - the official OS of the Raspberry Pi.

If you don't have a keyboard and a monitor hooked up to your Raspberry Pi, you might take a look at <a href="https://blokas.io/patchbox-os/docs/remote-control/" target="_blank">remote control options</a> for your Raspberry Pi. 

**In order to get the most from your Pisound you will need to install [The Pisound Software](the-pisound-software.md) to your Raspberry Pi.**
