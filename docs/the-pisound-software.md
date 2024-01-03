---
description: The Pisound software enables customizable button and the ability to control Pisound-based project vis The Pisound App.
---

# The Pisound Software

To gain access to all Pisound features, such as the customizable [button](the-button.md) or the ability to control Pisound-based projects via [The Pisound App](pisound-app.md), you need to [Install The Pisound Software](#install-the-pisound-software) (except if you are using <a href="https://blokas.io/patchbox-os/" target="_blank">Patchbox OS</a>).

**Note:** The Audio and MIDI functionality should work even without The Pisound Software installed, as the driver is integrated into the Linux Kernel.

Pisound is compatible with virtually all Linux distributions and software as it comes with an ALSA audio and MIDI driver integrated into mainline Raspberry Pi Linux kernel (ver. 4.4.27+).
The support driver for Pisound consists of two pieces - the Linux <a href="https://github.com/raspberrypi/linux/blob/rpi-4.19.y/sound/soc/bcm/pisound.c" target="_blank">kernel module</a> and <a href="https://github.com/BlokasLabs/pisound/tree/master/pisound-btn" target="_blank">user-space pisound-btn daemon</a>.

The Pisound button daemon is a user space program which implements monitoring of [The Button](the-button.md) on the board by registering a GPIO interrupt handler.
Therefore it takes minimal CPU resources, but is still able to react to button pushes just at the moment it was interacted with.

## Install The Pisound Software

To install the Pisound software on Debian compatible distributions like <a href="https://www.raspberrypi.org/downloads/raspberry-pi-os/" target="_blank">Raspberry Pi OS</a>, open a terminal (command) window and run:

```
curl https://blokas.io/pisound/install.sh | sh
```
This will set up the Blokas APT server and install all the software packages for Pisound. 

**Note:** It can also be used for updating the software.

Then run [`pisound-config`](pisound-config.md) to further customize your installation:

```
sudo pisound-config
```

