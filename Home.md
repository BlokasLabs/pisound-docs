![pisound-side](https://raw.githubusercontent.com/wiki/Pranciskus/wiki-test/images/pisound-side.png)
# Getting Started

pisound is a low latency high-quality soundcard and MIDI interface for Raspberry Pi pocket computer.

If you don't have any Linux OS running on your Raspberry Pi, we suggest starting with [Raspbian](https://www.raspberrypi.org/documentation/installation/installing-images/README.md).

Mount pisound on top of Raspberry Pi while the RPi is unpowered, so it appears as in the images at the top. [Power it up](#power-supply), install the driver by running the below commands in a terminal window:

```bash
wget http://blokas.io/pisound/install-pisound.sh -O install-pisound.sh
chmod +x install-pisound.sh
./install-pisound.sh
```
And reboot. Done! Thank You!

If you hit any issues with it, please see the [Step-by-Step pisound Installation Instructions](#manual-install).

## Print your own case

To take the setup process one step further, you can 3D-print your own case. All necessary files can be found here.

![pisound-case](https://raw.githubusercontent.com/wiki/Pranciskus/wiki-test/images/pisound-case.png)

# Hardware
The shield itself is designed for use with Raspberry Pi (RPi) computer exclusively and connects to it via the 40-pin header. The shield is slightly bigger in length (56x100 mm) than RPi itself. It has two female DIN-5 connectors for MIDI in/out and two 1/4" (6.35mm) stereo jack connectors for stereo audio in/out. There are two pots for gain and volume control, a programmable button and MIDI activity and input clip LEDs.

## Supported Raspberry Pi models

1. Raspberry Pi 1 Model A+
1. Raspberry Pi 1 Model B+
1. Raspberry Pi 2
1. Raspberry Pi 2 version 1.2
1. Raspberry Pi 3
1. Raspberry Pi Zero version 1.2/1.3

## Audio Input
There is one unbalanced stereo input accessible via 1/4" (6.35mm) jack slot on pisound. One stereo channel can also be used as two unbalanced mono channels. Audio inputs are AC coupled via metalized polypropylene capacitors to a gain stage built using [OPA4134](http://www.ti.com/lit/ds/symlink/opa2134.pdf) op-amps. Input resistance is 100kOhm for each channel. The gain can be adjusted simultaneously for both the left and the right channels from 0dB to +40dB with an on-board potentiometer. The maximum audio signal level before clipping is 5Vpp (at 0dB gain). The range of the gain adjustment can be divided into two sections. The first section occurs at rotation between 0% and 80% and it is used to precisely adjust for the high-level signals (line out, headphone amp out, etc...). The tight section at the maximum rotation of the gain pot acts as a +20dB switch for the low-level signals (guitar, microphone, etc.). When the signal clipping occurs in any channel, the red LED lights up and fades out after last clipped sample. Audio to digital conversion is carried out by [PCM1804](http://www.ti.com/lit/ds/symlink/pcm1804.pdf) converter. An on-board clock oscillator delivers a clock signal to the ADC, which divides it according to the selected sample rate. ADC acts as the master of I2S line. pisound supports three sample rates: 48kHz, 96kHz and 192kHz. A filter at the input stage of PCM1804 ensures good anti-[aliasing](https://en.wikipedia.org/wiki/Aliasing).

![pisound-side](https://raw.githubusercontent.com/wiki/Pranciskus/wiki-test/images/gain_vs_pot.jpg)
Gain and input sensitivity versus position of **GAIN** pot

## Audio Output
Audio output is DC coupled and can be accessed via the female 1/4" (6.35mm) stereo jack connector. Output volume level can be adjusted with on-board potentiometer. Maximum output level is 2.1Vrms when driving 1kOhm load. Digital to audio conversion is done in [PCM5102A](http://www.ti.com/lit/ds/symlink/pcm5100a-q1.pdf) converter which has both Signal-to-Noise ratio and dynamic range of 112dB. The DAC acts as a slave on I2S line and the sampling rate is dictated by the ADC. An intelligent muting system is used to prevent pops and clicks when disconnecting power supply.

## Audio Latency and Other Parameters

In digital audio equipment it takes time for the signal at input to be processed and delivered at output. This time is called audio latency. There's three parts to it. The first is the time required for the ADC to do the conversion and to send digital data to the processing unit. The second step is to process data and prepare it for the transfer to the DAC. And the final part is getting the data to the DAC and converting it to the analog signal. The most time consuming is the second part. Parts one and three often require no more than 1 ms depending on architecture of ADC and DAC, sample rate and in-built digital filters.

![audio latency of 4.941 ms](https://raw.githubusercontent.com/wiki/Pranciskus/wiki-test/images/48kHz_audio_latency.png)

An oscillogram showing audio latency of 4.941 ms. pisound and Raspberry Pi 2 working at sample rate of 48 kHz

![spectrum of looped white noise](https://raw.githubusercontent.com/wiki/Pranciskus/wiki-test/images/48kHz_BW.jpg)
A spectrum of looped white noise signal showing the bandwidth of the pisound

![spectrum of looped 1 kHz sine signal](https://raw.githubusercontent.com/wiki/Pranciskus/wiki-test/images/48kHz_THD.jpg)
A spectrum of looped 1 kHz sine signal showing the THD of the pisound

## MIDI Interface

A standard MIDI interface is available via two female DIN-5 connectors. MIDI on pisound is implemented using high speed SPI and a dedicated MCU for translating SPI data to serial MIDI byte streams and it's readily recognized in audio software as an ALSA Raw MIDI device. Such implementation compared to UART based solutions for MIDI communication on RPi has the advantages of keeping the kernel configuration simple (the required 31250Hz UART baud rate is not considered standard for terminals, so it is complex to actually configure the serial device to run at the baud rate required by MIDI) and additionally it is exposed to the OS as an ALSA Raw MIDI device, rather than a serial terminal device which is ignored by most if not all software that interacts with MIDI.

![signal delay between MIDI](https://raw.githubusercontent.com/wiki/Pranciskus/wiki-test/images/midi_latency.png)

An oscillogram showing signal delay between MIDI input (yellow) and MIDI output (cyan) of 2.105 ms when echoing

## Unused Pins of RPi Header
![pinout map rev3](https://raw.githubusercontent.com/wiki/Pranciskus/wiki-test/images/pisound-pins.png)

* Black - Power supply pins.
* Red - Pins used by pisound.
* Green - Pins available for your use.
* Blue - Pins reserved for Raspberry Pi hats use.

## Power Supply 
There are two versions of pisound regarding power supply:

* **9V version:** 7.2V - 12.6V, 18W minimum. 5.5x2.1 mm coaxial power jack connector. The inner connector is connected to the positive terminal, and the sleeve is connected to the ground. The power adapter connected to pisound supplies the RPi board too, so RPi does not need to have its USB supply port connected. The pisound itself has a power consumption of about 1.8W. A 9VDC power supply capable of delivering at least 2 Amps of current is recommended for this version.

* **5.1V version:** pisound has no power connection and requires no additional power supply. It powers up from RPi power supply via pins on RPi header. pisound consumes no more than 300mA at 5.1VDC. When using this version of pisound, we recommend to use the official [5.1VDC RPi power supply](https://www.raspberrypi.org/products/universal-power-supply/).

## Specs
**Parameter**|**Conditions**|**Value**
:-----|:-----|:-----
Sampling frequency (Fs)|-|48kHz, 96kHz, 192kHz
Input/Output resolution|-|24bit
Input/Output SNR@1kHz|G = 0 dB|110dB
Input impedance|-|100kOhm II 2pF
Input gain (G)|-|0dB to +40dB
Input clip voltage|G = 0 dB|5V (peak to peak)
Full scale output|Load impedance > 1 kOhm|0V to 2.1V (RMS)
Loopback bandwidth (-3 dB)|G = 0 dB, Fs = 48 kHz|7.5Hz - 23kHz
Loopback THD@1kHz|G = 0 dB, Fs = 48 kHz|< 0.045%
Loopback latency|Fs = 192 kHz, Rpi3, buffer size = 128 frames|2.092ms
MIDI loopback latency|-|2.105ms
Current Draw|Raspberry Pi-dedicated power supply|< 300mA @ 5.1VDC
Dimensions|-|56mm x 100mm
Weight|-|67g
Phantom power|-|None

# Drivers
The support software for pisound consists of two pieces - the Linux kernel module and user-space pisound-btn daemon. The driver was tested only on Raspbian distribution, but it may work on other distributions capable of running on RPi. If you get it to run on a distro which is not listed below, please add it to the list, and if any special actions were required, create a page about it and link to it.

The kernel module implements the soundcard as ALSA Input / Output / Raw MIDI device.

The pisound button daemon is a user space program which implements monitoring of The Button on the board by registering a GPIO interrupt handler. Therefore it takes the minimal CPU resources, but is still able to react to button pushes just at the moment it was interacted with. Read more on [The Button]([#the-button) functionality below.

You can find the source code [here](https://github.com/BlokasLabs/pisound/).

## Note on Kernel Upgrades
As Linux kernel modules must be built with the kernel headers matching the same version of the kernel, that means that modules must be rebuilt for each new kernel version on the system.

In layman's terms, that basically means each time your kernel version changes, you should reinstall the kernel module by following the install instructions, or, if you still have the source code checked out, do:
```bash
rpi-source
cd pisound
make clean
make
sudo make install
```

## Known Compatible Distributions

Please add or let us know if you have pisound working on a distribution that is not in the list yet!

* [Raspbian](https://www.raspbian.org/ )

## Linux Driver

To install the user-space button daemon and enable pisound, run the below commands in a terminal.

Alternatively, if you hit any issues with it, please see the [Step-by-Step pisound Installation Instructions](#manual-install) and send us feedback!
```bash
wget http://blokas.io/pisound/install-pisound.sh -O install-pisound.sh
chmod +x install-pisound.sh
./install-pisound.sh
```

The above installs a button daemon named 'pisound-btn' and its scripts for button actions. If there were existing scripts, they are first backed up to `/usr/local/etc/pisound/backups/<current date>`, so if you had overridden the default functions, you will have to restore your scripts manually.

The install-pisound.sh script does the following steps:

1. Runs rpi-update to update your kernel version. Versions 4.4.27+ contain the pisound kernel module by default.
1. Downloads the [pisound source code from github](https://github.com/BlokasLabs/pisound/), and puts it in **pisound** folder, relative to the current working directory (usually the home folder).
1. Builds and installs the pisound software.
1. Enables the pisound device-tree overlay, so the sound card is recognized by RPi.

If you want to disable the card, you can do that by using a terminal:
```bash
cd pisound
sudo ./disable-pisound.sh
```

## Manual Install

1. Start a terminal.

2. Update your kernel (not necessary if your kernel is already 4.4.42 or newer)
```
 sudo rpi-update
```
3. Download pisound's source code:
```
 git clone https://github.com/BlokasLabs/pisound.git
```
4. Build and install pisound-btn:
```
 cd pisound/pisound-btn
 make
 sudo make install
```
5. Enable pisound's device tree overlays:
```
 chmod +x ../enable-pisound.sh
 sudo ../enable-pisound.sh
```
6. Reboot the system:
```
 sudo reboot
```


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
You should expect to see items with 'pisound' listed.

### Feedback
In case you're having difficulties with getting pisound's driver to run, contact us and the community http://community.blokas.io/, provide the exact error and the last command you've executed.

## The Button

The Button is a customizable button on the pisound board. There's 4 possible interactions with it:

1. Press, hold and release after 1 or more seconds passed. Executes `/usr/local/etc/pisound/hold.sh`
1. Click once. Executes `/usr/local/etc/pisound/single_click.sh`
1. Double click. Executes `/usr/local/etc/pisound/double_click.sh`
1. Triple click. Executes `/usr/local/etc/pisound/triple_click.sh`

Every **down** and **up** movement will also execute `/usr/local/etc/pisound/down.sh` and `/usr/local/etc/pisound/up.sh` respectively.

You may modify these scripts to do whatever you wish.

**Note:** The pisound-btn daemon process must be running in order for the button to work. The 'make install' automatically adds it to the Desktop autostart.

### Default Hold Action
By default, holding the button down for more than 1 second and releasing will cause the system to shutdown cleanly. The MIDI activity LEDs will blink once to confirm that the hold action was triggered. It is safe to unplug the power supply once RPi's LEDs stop flashing on and off.

### Default Single Click Action

By default, clicking the button once runs a script that scans the attached media storage devices for '**main.pd**', or, if not found in external media, scans `/usr/local/etc/pisound-patches/`, kills all existing instances of Pure Data, then starts a new instance opening the file, enables audio and connects all the detected MIDI inputs and outputs to the Pure Data's virtual MIDI ports. ALSA engine of Pure Data is used.

The script will blink the MIDI Activity LEDs once just after it reacted to the click, and again 2 times after it succeeded launching the patch, or one long duration blink if an error occurred or no patch was found.

If you don't have Pure Data installed, chances are you can install it by running:
```bash
sudo apt-get install puredata
```

### Default Double Click Action

Double clicking will stop all Pure Data instances and unmount all attached external media, so it can be safely removed.

### Default Triple Click Action (RPi3 or using SoftAP capable WiFi USB adapter)

Triple-clicking will reconfigure the WiFi of RPi3 board or an external SoftAP capable USB WiFi adapter to behave as an Access Point (a.k.a. Wireless Router), as well as start 'touchosc2midi' monitor which will be ready to listen and forward MyOsc data as MIDI to other software such as puredata.

By default, the AP will appear as '**pisound**', and the default password is '**blokaslabs**' (without quotes). You can change the name and password by modifying `/usr/local/etc/pisound/hostapd.conf` Keep in mind that this file gets backed up and overwritten after reinstalling or updating the pisound-btn software.

The default IP address of RPi3 is 172.24.1.1 which you can use for ssh, VNC or wireless OSC / MIDI data! That means that you can easily interact with your system using just your laptop or phone, no more wires apart from power supply is needed! And it gets better, if the LAN cable is connected to RPi, it will share the Internet with the connected devices.

To access RPi using ssh and/or VNC, make sure they're enabled in `raspi-config`. To enable, run in terminal:
```bash
sudo raspi-config
```
Go to **Interfacing Options** and make sure **ssh** and **VNC** are enabled. If you don't see these options, go to **Advanced Options** and do **Update**.

Triple-clicking again will revert to regular WiFi behavior.

# Compatible Software

pisound is compatible with virtually all Linux distributions and software as it comes with an ALSA audio and MIDI driver integrated in mainline Raspbian Linux kernel (ver. 4.4.27+). 

Tested programs in alphabetic order:

* Audacity
* Carla
* Jack
* Supercollider
* Pure Data

## Sonic Pi workaround guide
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