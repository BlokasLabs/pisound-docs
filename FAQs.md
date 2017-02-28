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


## How to send WiFi-MIDI messages to your Raspberry Pi?

Making use of your phone or a tablet for controlling software on Raspberry Pi using pisound is relatively easy, just follow these steps:

1. Triple-click the button on pisound to enable the WiFi hotspot mode, touchosc2midi starts also.
1. Connect to "pisound" WiFi access point using a phone or a tablet, using 'blokaslabs' as the password.
1. Open TouchOSC, MyOSC or similar app on your external device.
1. Host IP address in the app's settings should be 172.24.1.1
1. That's it. From this point you can send messages from your phone/tablet to software running on your Raspberry Pi. The MIDI CC / Note number depends on the configuration of the app, for more information, see the TouchOSC documentation here: https://hexler.net/docs/touchosc-editor-controls-properties

**Note:** If you want to to control Pd patch with Wifi-MIDI messages, after completing the steps above, insert a USB stick with your patch files into your Raspberry Pi and click the pisound button once to launch the patch!