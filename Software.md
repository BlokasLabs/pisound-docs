# Software

pisound is compatible with virtually all Linux distributions and software as it comes with an ALSA audio and MIDI driver integrated in mainline Raspbian Linux kernel (ver. 4.4.27+). The support driver for pisound consists of two pieces - the Linux kernel module and user-space pisound-btn daemon.

The pisound button daemon is a user space program which implements monitoring of The Button on the board by registering a GPIO interrupt handler. Therefore it takes minimal CPU resources, but is still able to react to button pushes just at the moment it was interacted with. Read more on [The Button]([#the-button) functionality below.

You can find the source code for The Button [here](https://github.com/BlokasLabs/pisound/) and kernel module [here](https://github.com/raspberrypi/linux/blob/rpi-4.9.y/sound/soc/bcm/pisound.c).

## Installing The Driver

To install the user-space button daemon and enable pisound, run the below commands in a terminal.

```bash
wget http://blokas.io/pisound/install-pisound.sh -O install-pisound.sh
chmod +x install-pisound.sh
./install-pisound.sh
```

The above installs a button daemon named 'pisound-btn' and its scripts for button actions. If there were existing scripts, they are first backed up to `/usr/local/etc/pisound/backups/<current date>`, so if you had overridden the default functions, you will have to restore your scripts manually.

### Verifying It Works

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
In case you're having difficulties with getting pisound's driver to run, contact us and the community here: http://community.blokas.io/, provide the exact error and the last command you've executed.

## Compatible Software

Please add or let us know if you have pisound working on a distribution that is not in the list yet!

* [Raspbian](https://www.raspbian.org/)
* [arch linux](https://www.archlinux.org/)
* [Ubuntu Mate](https://ubuntu-mate.org/raspberry-pi/)

Software we tested which required no special changes, in alphabetic order:

* Audacity
* Carla / LV2
* darkice and icecast
* Jack
* Supercollider
* Pure Data

Software which required tweaks or workarounds, in alphabetic order:

* Sonic Pi
* [Volumio](http://community.blokas.io/t/setting-up-volumio/81)

### Starting Jack Server for pisound

In case you don't have qjackctl installed, run:
```
sudo apt-get install qjackctl
```
To start the server, configured to use pisound, follow these steps:

1. Run QJackCtl (either from the Applications->Sound & Video menu or executing 'qjackctl' in a terminal.
1. Click Setup...
1. Make sure that the following settings are set correctly:
  1. Sample Rate is one of '48000', '96000' or '192000'. (The higher the sample rate, the more there is data to process for software. However, higher sample rate has slightly less latency, if software is fast enough to keep up!)
  1. Input Device and Output Device both are set to 'hw:pisound'.
  1. MIDI Driver is set to 'seq'
1. Close the Setup window.
1. Click Start.
1. If everything is fine, you should see 'Started' text written at the top left corner of the status area.

**Note:** If something goes wrong, try different settings in Setup, and make sure that the sound card is not being exclusively used by some other software.

![jack-setup](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/jack_setup.png)


### Sonic Pi Workaround Guide
Sonic Pi has a special case for Raspberry Pi during startup. By default it kills the existing jackd server, and starts one configured to use the built in Raspberry Pi audio with hardcoded parameters. That makes Sonic Pi unusable with other audio cards, unless the below workaround is applied, so Sonic Pi would behave in the same way as if it's run on any other linux device.

Do this once (might need to be done again in case any software updates touch it):

- Edit `/opt/sonic-pi/app/server/sonicpi/scsynthexternal.rb` as root user using your favorite editor (such as vim, nano, geany, or ...).
- Locate the following code snippet:
```
     case os
     when :raspberry
       boot_server_raspberry_pi
     when :linux
       boot_server_linux
     when :osx
       boot_server_osx
     when :windows
       boot_server_windows
     end
     true
```
- Change it to:
```
     case os
     when :raspberry
       #boot_server_raspberry_pi
       boot_server_linux
     when :linux
       boot_server_linux
     when :osx
       boot_server_osx
     when :windows
       boot_server_windows
     end
     true
```

Do this every time you want to start Sonic Pi:

- Start Jack server configured to use pisound.
- Start Sonic Pi, it will connect to the Jack server you had just launched.
