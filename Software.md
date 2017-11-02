# Software

Pisound is compatible with virtually all Linux distributions and software as it comes with an ALSA audio and MIDI driver integrated into mainline Raspbian Linux kernel (ver. 4.4.27+). The support driver for Pisound consists of two pieces - the Linux kernel module and user-space pisound-btn daemon.

The Pisound button daemon is a user space program which implements monitoring of The Button on the board by registering a GPIO interrupt handler. Therefore it takes minimal CPU resources, but is still able to react to button pushes just at the moment it was interacted with. Read more on [The Button](the-button) functionality below.

You can find the source code for The Button [here](https://github.com/BlokasLabs/pisound/tree/master/pisound-btn) and kernel module [here](https://github.com/raspberrypi/linux/blob/rpi-4.9.y/sound/soc/bcm/pisound.c).

## Installing/Updating The Pisound Software

To install the Pisound software, run the below commands in a terminal.

```bash
curl https://blokas.io/pisound/install-pisound.sh | sh
```

This will set up the Blokas APT server and install all the software packages for Pisound. Then you may run [`pisound-config`](pisound-config) to further customize your installation:

```bash
sudo pisound-config
```

## Verifying It Works

Once the system boots up, run a terminal and run:
```
 amidi -l
 arecord -l
 aplay -l
```
You should see output similar to:
```
 pi@raspberrypi:~ $ amidi -l
 Dir Device    Name
 IO  hw:1,0    pisound MIDI PS-10JAD4Q
 pi@raspberrypi:~ $ arecord -l
 **** List of CAPTURE Hardware Devices ****
 card 1: pisound [pisound], device 0: PS-10JAD4Q snd-soc-dummy-dai-0 []
   Subdevices: 1/1
   Subdevice #0: subdevice #0
 pi@raspberrypi:~ $ aplay -l
 **** List of PLAYBACK Hardware Devices ****
 card 0: ALSA [bcm2835 ALSA], device 0: bcm2835 ALSA [bcm2835 ALSA]
   Subdevices: 8/8
   Subdevice #0: subdevice #0
   Subdevice #1: subdevice #1
   Subdevice #2: subdevice #2
   Subdevice #3: subdevice #3
   Subdevice #4: subdevice #4
   Subdevice #5: subdevice #5
   Subdevice #6: subdevice #6
   Subdevice #7: subdevice #7
 card 0: ALSA [bcm2835 ALSA], device 1: bcm2835 ALSA [bcm2835 IEC958/HDMI]
   Subdevices: 1/1
   Subdevice #0: subdevice #0
 card 1: pisound [pisound], device 0: PS-10JAD4Q snd-soc-dummy-dai-0 []
   Subdevices: 1/1
   Subdevice #0: subdevice #0
```

### Feedback
In case you're having difficulties with getting Pisound to run, contact us and the community [here](https://community.blokas.io/), provide the exact error and the last command you've executed.

## Compatible Software

Please add or let us know if you have Pisound working on a distribution that is not on the list yet!

* [Raspbian](https://www.raspbian.org/)
* [arch linux](https://www.archlinux.org/)
* [Ubuntu Mate](https://ubuntu-mate.org/raspberry-pi/)
* [Volumio](https://volumio.org/)

The software we tested which required no special changes, in alphabetic order:

* Audacity
* Carla / LV2
* darkice and icecast
* Jack
* Pure Data
* Sonic Pi
* Supercollider