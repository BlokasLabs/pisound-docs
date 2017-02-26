# Projects using pisound

## pisound and LV2 plugins
On http://rpi.autostatic.com/ repository you can find some cool software packages for audio. One of those is **Carla** - a host for LV2 plugins.

![Carla](https://raw.githubusercontent.com/wiki/Pranciskus/wiki-test/images/carla.png)

So let's give it a go.

First we have to add the repository to apt-get:
```
wget -q -O - http://rpi.autostatic.com/autostatic.gpg.key | sudo apt-key add -
sudo wget -q -O /etc/apt/sources.list.d/autostatic-audio-raspbian.list http://rpi.autostatic.com/autostatic-audio-raspbian.list
sudo apt-get update
```
After that's done, let's install **Carla** as well as **jackd** server and **qjackctl** for configuring it:
```
sudo apt-get install carla jackd qjackctl
```
Then start **qjackctl**, you can do that by hitting Ctrl+F2 and typing in **qjackctl** and pressing enter.

Configure it in Setup... like so:

![Jack Settings](https://raw.githubusercontent.com/wiki/Pranciskus/wiki-test/images/jack_settings.png)

And start the jack server by pressing Start:

![Jack Start](https://raw.githubusercontent.com/wiki/Pranciskus/wiki-test/images/start_jack.png)

Now run Carla (Alt+F2, carla, enter)

Click Configure Carla and make sure that Audio Driver is set to JACK. It will connect to the already started Jack server. For process mode I've used Single Client, but you can use any option you like.

Then you can add plugins in Plugins section, and patch them up in Patch Bay section.

The top screenshot shows **ZSynAddSubFX** connected to pisound's Input and Output, as well as MIDI data routed to the synth plugin.

There's a couple of plugins readily available to play around within Carla, and you should be able to find more LV2 plugins built for Raspberry Pi to experiment with on the Internet.

## Internet Radio Station

Using **icecast** and **darkice** you can easily setup your own radio station and broadcast whatever is connected to the pisound input!

First let's get the required software installed:
```
sudo apt-get install icecast2 darkice
```
Then we have to configure the darkice, feel free to customize to your liking.

Save the below contents to `/etc/darkice.cfg`
```
# this section describes general aspects of the live streaming session
[general]
duration      = 0                # duration of encoding, in seconds. 0 means forever
bufferSecs    = 2                # size of internal slip buffer, in seconds
reconnect     = yes              # reconnect to the server(s) if disconnected

# this section describes the audio input that will be streamed
[input]
device        = hw:1,0           # Alsa soundcard device for the audio input
sampleRate    = 48000            # sample rate in Hz. try 48000, 96000 or 192000
bitsPerSample = 16               # bits per sample. try 16
channel       = 2                # channels. 1 = mono, 2 = stereo.
                                 # Only stereo mode is supported by pisound.

# this section describes a streaming connection to an IceCast2 server
# there may be up to 8 of these sections, named [icecast2-0] ... [icecast2-7]
# these can be mixed with [icecast-x] and [shoutcast-x] sections
[icecast2-0]
bitrateMode   = cbr              # variable bit rate
bitrate       = 128
format        = mp3              # format of the stream: mp3
quality       = 0.8              # quality of the stream sent to the server
server        = localhost        # host name of the server
port          = 8000             # port of the IceCast2 server, usually 8000
password      = hackme           # source password to the IceCast2 server
mountPoint    = pisound          # mount point of this stream on the IceCast2 server
name          = pisound          # name of the stream
description   = DarkIce on pisound # description of the stream
url           = http://localhost # URL related to the stream
genre         = my genre         # genre of the stream
public        = no               # advertise this stream?
#localDumpFile = recording.mp3   # Record also to a file
```

And finally, execute 'darkice' in a terminal or using Alt+F2:

`darkice`

Then from other devices you can connect to `http://raspberrypi_ip:8000/` to see generic information about the station and `http://raspberry_pi:8000/pisound` to listen. (Replace raspberrypi_ip in the URLs using the IP of the Raspberry Pi)

For your station to be reachable outside of your local network, you need to have an externally accessible IP address provided by your ISP and you need to configure port forwarding on your router to forward requests on some port to port 8000 on Raspberry Pi. However, this is out of scope for this guide, there should be plenty of info around on how to set that up.

## Network enabled Hi-Fi player
You can use pisound with Volumio! Though they're still running on an older kernel which was before the pisound driver was integrated, so some manual tweaks are still necessary. Our changes to integrate pisound are still pending on them updating their kernel, you can express your interest here: https://github.com/volumio/Volumio2/pull/955

**Eventually none of this manual setup is going to be necessary, as soon as Volumio makes a release with an updated kernel.**

![Volumio](https://raw.githubusercontent.com/wiki/Pranciskus/wiki-test/images/volumio.png)

Steps to get started:

1. Download latest Volumio for Raspberry Pi: https://volumio.org/get-started.
1. Flash the image to an SD card, as usual.
1. Boot it.
1. SSH to volumio@volumio.local (or use the IP directly after @), password is volumio.
  - It may take a while after booting until SSH is ready to receive connections.
1. Download kernel source code using:
  - `volumio kernelsource`
1. The current volumio kernelsource seems to fail before adjusting the kernel version in the sources, but we can fix it up ourselves:
  - `sudo nano /usr/src/rpi-linux/include/config/kernel.release`
  - Make sure there's a '+' at the end of the version, so it looks like so: `4.4.9-v7+`
  - Press ctrl+x, y and enter to save and exit.
1. Download the pisound driver code:
  - `git clone https://github.com/BlokasLabs/pisound`
1. Build and install it:
  - `cd pisound/pisound-module`
  - `sudo make install`
1. Add pisound to Volumio's DAC selection:
  - `sudo nano /volumio/app/plugins/system_controller/i2s_dacs/dacs.json`
  - Add a line somewhere in the middle like so: `{"id":"pisound","name":"pisound","overlay":"pisound","alsanum":"1","mixer":"","modules":"","script":"","needsreboot":"no"},`
  - Note that a comma after this line is necessary, unless it's the last entry in the scope (before }), if there's a syntax error in this file, Volumio won't be able to show playback option in its web interface.
  - Save and exit using ctrl+x, y and enter.
1. Open http://volumio.local on your PC, go to Settings -> Playback Options.
1. Enable I2S DAC.
1. Select 'pisound'
1. Click save. As it saves the configuration and starts the card, the MIDI activity LEDs should flash briefly.
1. Change Mixer Type to 'Software'.
1. Done! Thank you! Now you can enjoy using pisound as a network media player!

## pisound with DIY MIDI controller

If you want to add more controls to your pisound audio projects, you can make your own MIDI controller using a couple of potentiometers, push buttons and any Arduino compatible board that uses ATmega32U4 microcontroller. Alternatively, it should be straightforward to adapt the example code to use DIN-5 MIDI ports, as the usbmidi API interface is compatible with Serial API.

![DIY MIDI controller](https://raw.githubusercontent.com/wiki/Pranciskus/wiki-test/images/diy-midi-ctrl.png)

Go [here](https://github.com/BlokasLabs/usbmidi/tree/master/arduino/libraries/usbmidi/examples/midictrl) for full instructions.