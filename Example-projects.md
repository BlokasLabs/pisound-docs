# Projects Using Pisound

## Pisound and LV2 Plugins
On http://rpi.autostatic.com/ repository you can find some cool software packages for audio. One of those is **Carla** - a host for LV2 plugins.

![Carla](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/carla.png)

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
Then start **qjackctl**, you can do that by hitting Alt+F2 and typing in **qjackctl** and pressing enter.

Configure it in Setup... like so:

![Jack Settings](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/jack_settings.png)

And start the jack server by pressing Start:

![Jack Start](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/start_jack.png)

Now run Carla (Alt+F2, carla, enter)

Click Configure Carla and make sure that Audio Driver is set to JACK. It will connect to the already started Jack server. For process mode I've used Single Client, but you can use any option you like.

Then you can add plugins in Plugins section, and patch them up in Patch Bay section.

The top screenshot shows **ZSynAddSubFX** connected to Pisound's Input and Output, as well as MIDI data routed to the synth plugin.

There's a couple of plugins readily available to play around within Carla, and you should be able to find more LV2 plugins built for Raspberry Pi to experiment with on the Internet.

## Internet Radio Station

Using **icecast** and **darkice** you can easily setup your own radio station and broadcast whatever is connected to the Pisound input!

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
                                 # Only stereo mode is supported by Pisound.

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
description   = DarkIce on Pisound # description of the stream
url           = http://localhost # URL related to the stream
genre         = my genre         # genre of the stream
public        = no               # advertise this stream?
#localDumpFile = recording.mp3   # Record also to a file
```

And finally, execute 'darkice' in a terminal or using Alt+F2:

`darkice`

Then from other devices you can connect to `http://raspberrypi_ip:8000/` to see generic information about the station and `http://raspberry_ip:8000/pisound` to listen. (Replace raspberrypi_ip in the URLs using the IP of the Raspberry Pi)

For your station to be reachable outside of your local network, you need to have an externally accessible IP address provided by your ISP and you need to configure port forwarding on your router to forward requests on some port to port 8000 on Raspberry Pi. However, this is out of scope for this guide, there should be plenty of info around on how to set that up.

## Network Enabled Hi-Fi Player
You can use Pisound with Volumio! Since version 2.129 (25-03-2017), Pisound's module is integrated into Volumio, so installing the latest version or updating should be enough to get 'pisound' listed in Playback Options. Just enable I2S DACs, pick Pisound and save the configuration! We recommend switching the mixer to 'Software', if you want to control the volume within Volumio. You can use 'Hardware' mixer if using the physical volume control on Pisound.

Now you can enjoy using Pisound as a network media player!

![Volumio](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/volumio.png)

## Pisound with DIY MIDI Controller

If you want to add more controls to your Pisound audio projects, you can make your own MIDI controller using a couple of potentiometers, push buttons and any Arduino compatible board that uses ATmega32U4 microcontroller. Alternatively, it should be straightforward to adapt the example code to use DIN-5 MIDI ports, as the usbmidi API interface is compatible with Serial API.

![DIY MIDI controller](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/diy-midi-ctrl.png)

Go [here](https://github.com/BlokasLabs/usbmidi/tree/master/arduino/libraries/usbmidi/examples/midictrl) for full instructions.