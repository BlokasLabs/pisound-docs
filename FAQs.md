## How to start Jack Server for pisound?

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


## How to use Sonic Pi with pisound?

The latest version of Sonic Pi works out of the box without any manual changes! Just start it from the menu -> Programming -> Sonic Pi!

## How to send WiFi-MIDI messages to your Raspberry Pi?

Making use of your phone or a tablet for controlling software on Raspberry Pi using pisound is relatively easy, just follow these steps:

1. Triple-click the button on pisound to enable the WiFi hotspot mode, touchosc2midi starts also.
1. Connect to "pisound" WiFi access point using a phone or a tablet, using 'blokaslabs' as the password.
1. Open TouchOSC, MyOSC or similar app on your external device.
1. Host IP address in the app's settings should be 172.24.1.1
1. That's it. From this point you can send messages from your phone/tablet to software running on your Raspberry Pi. The MIDI CC / Note number depends on the configuration of the app, for more information, see the TouchOSC documentation here: https://hexler.net/docs/touchosc-editor-controls-properties

**Note:** If you want to to control Pd patch with Wifi-MIDI messages, after completing the steps above, insert a USB stick with your patch files into your Raspberry Pi and click the pisound button once to launch the patch!