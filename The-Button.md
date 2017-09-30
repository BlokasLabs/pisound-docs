# The Button

The Button is a customizable button on the Pisound board. There's a lot of mappable actions for it. The mapping is expressed in [`/etc/pisound.conf`](https://github.com/BlokasLabs/pisound/blob/master/pisound-btn/pisound.conf):

| Action Id | Description | Default script* |
| --- | --- | --- |
| DOWN | Button gets pushed down. | [down.sh](https://github.com/BlokasLabs/pisound/blob/master/pisound-btn/scripts/down.sh) |
| UP | Button gets released up. | [up.sh](https://github.com/BlokasLabs/pisound/blob/master/pisound-btn/scripts/up.sh) |
| SINGLE_CLICK | Button was clicked once. | [start_puredata.sh](https://github.com/BlokasLabs/pisound/blob/master/pisound-btn/scripts/start_puredata.sh) |
| DOUBLE_CLICK | Button was double-clicked. | [stop_puredata.sh](https://github.com/BlokasLabs/pisound/blob/master/pisound-btn/scripts/stop_puredata.sh) |
| TRIPLE_CLICK | Button was triple-clicked. | [toggle_wifi_hotspot.sh](https://github.com/BlokasLabs/pisound/blob/master/pisound-btn/scripts/toggle_wifi_hotspot.sh) |
| OTHER_CLICKS | Button was clicked between 4 and 8 times. | [click.sh](https://github.com/BlokasLabs/pisound/blob/master/pisound-btn/scripts/click.sh) |
| HOLD_1S | Button was held down between 0.4s and 3s. | [hold.sh](https://github.com/BlokasLabs/pisound/blob/master/pisound-btn/scripts/hold.sh) |
| HOLD_3S | Button was held down between 3s and 5s. | [toggle_bt_discoverable.sh](https://github.com/BlokasLabs/pisound/blob/master/pisound-btn/scripts/toggle_bt_discoverable.sh) |
| HOLD_5S | Button was held down between 5s and 7s. | [shutdown.sh](https://github.com/BlokasLabs/pisound/blob/master/pisound-btn/scripts/shutdown.sh) |
| HOLD_OTHER | Button was held down for more than 7s. | [shutdown.sh](https://github.com/BlokasLabs/pisound/blob/master/pisound-btn/scripts/shutdown.sh) |

\* All default scripts are stored at `/usr/local/etc/pisound/`

You may modify these scripts to do whatever you wish or re-map any of the action to execute your own custom script. Keep in mind that whenever you install / update the pisound-btn, the existing scripts stored at `/usr/local/etc/pisound/` are first backed up to `/usr/local/etc/pisound/backups/<current date>`, so if you had overridden the default scripts, you will have to restore your scripts manually.

**Note:** The pisound-btn daemon process must be running in order for the button to work. The 'make install' automatically adds it to systemd autostart.

## Mapping the Actions

You may re-map the default actions or point them to your own custom scripts to be executed by modifying the [`/etc/pisound.conf`](https://github.com/BlokasLabs/pisound/blob/master/pisound-btn/pisound.conf).

The config file format allows placing comments by prefixing the line with # character. Comments must begin at the first word of the line.

Non-comment lines must consist of two space separated values. Spaces in the value are not permitted, therefore the absolute path to a custom script must not contain any spaces.

```
# Example comment
VARIABLE value_without_spaces
```

## Main Action Scripts

### [`shutdown.sh`](https://github.com/BlokasLabs/pisound/blob/master/pisound-btn/scripts/shutdown.sh) - Safely Shutdown The OS
By default, holding the button down for more than 5 seconds and releasing will cause the system to shutdown cleanly by executing ```sudo shutdown now```. The MIDI activity LEDs will blink ten times quickly to confirm that the hold action was triggered. It is safe to unplug the power supply once RPi's activity LEDs stop flashing on and off. To restart, unplug and plug the power in again.

### [`start_puredata.sh`](https://github.com/BlokasLabs/pisound/blob/master/pisound-btn/scripts/start_puredata.sh) - Start a Pure Data Patch

By default, clicking the button once runs a script that scans the attached media storage devices for '**main.pd**', or, if not found in external media, scans `/usr/local/etc/pisound-patches/`, kills all existing instances of Pure Data, then starts a new instance opening the file, enables audio and connects all the detected MIDI inputs and outputs to the Pure Data's virtual MIDI ports. ALSA engine of Pure Data is used.

The script will blink the MIDI Activity LEDs once just after it reacted to the click, and again 2 times after it succeeded launching the patch, or one long duration blink if an error occurred or no patch was found.

### [`stop_puredata.sh`](https://github.com/BlokasLabs/pisound/blob/master/pisound-btn/scripts/stop_puredata.sh) - Turn Off Pure Data & Eject USB

Double clicking will stop all Pure Data instances and unmount all attached external media, so it can be safely removed.

### [`toggle_wifi_hotspot.sh`](https://github.com/BlokasLabs/pisound/blob/master/pisound-btn/scripts/toggle_wifi_hotspot.sh) - Toggle WiFi Hotspot Mode

Triple-clicking will reconfigure the WiFi of RPi3 board or an external SoftAP capable USB WiFi adapter to behave as an Access Point (a.k.a. Wireless Router), as well as start 'touchosc2midi' monitor which will be ready to listen and forward MyOsc data as MIDI to other software such as Pure Data.

By default, the AP will appear as '**pisound**', and the default password is '**blokaslabs**' (without quotes). You can change the name and password by modifying `/usr/local/etc/pisound/hostapd.conf` Keep in mind that this file gets backed up and overwritten after reinstalling or updating the pisound-btn software.

The default IP address of RPi in WiFi Hotspot mode is 172.24.1.1 which you can use for ssh, VNC or wireless OSC / MIDI data! That means that you can easily interact with your system using just your laptop or phone, no more wires apart from power supply is needed! And it gets better, if the LAN cable is connected to RPi, it will share the Internet with the connected devices over WiFi!

To send MIDI OSC messages from your other devices to Pisound, connect to the Pisound's WiFi network, and set the 172.24.1.1 IP as the host in the software you're using (such as MyOSC or TouchOSC) settings. See [here](faqs#how-to-send-wifi-midi-messages-to-your-raspberry-pi) for more information.

To access RPi using ssh and/or VNC, make sure they're enabled in `raspi-config`. To enable, run in terminal:
```bash
sudo raspi-config
```
Go to **Interfacing Options** and make sure **ssh** and **VNC** are enabled. If you don't see these options, go to **Advanced Options** and do **Update**.

Triple-clicking again will revert to regular WiFi behavior.

### [`toggle_bt_discoverable.sh`](https://github.com/BlokasLabs/pisound/blob/master/pisound-btn/scripts/toggle_bt_discoverable.sh) - Toggle Bluetooth Discoverability on and Off

Holding The Button between 3 and 5 seconds will toggle the Bluetooth Discoverability. See [Pisound App](pisound-app) for controlling Pisound & RPi remotely.

## Miscellaneous Scripts

### [`hold.sh`](https://github.com/BlokasLabs/pisound/blob/master/pisound-btn/scripts/hold.sh)
A placeholder script for hold actions. It gets two arguments - first one for the count of clicks after which the button was held for a longer time and the second for the hold duration.
### [`down.sh`](https://github.com/BlokasLabs/pisound/blob/master/pisound-btn/scripts/down.sh)
A script that gets triggered every time The Button is pushed down. By default it has code to start periodic LED flashing, in case The Button is going to remain held down.
### [`up.sh`](https://github.com/BlokasLabs/pisound/blob/master/pisound-btn/scripts/up.sh)
The counter part of `down.sh` - it gets triggered every time The Button is released and by default it is stopping the LED flashing initiated in the down event.
### [`common.sh`](https://github.com/BlokasLabs/pisound/blob/master/pisound-btn/scripts/common.sh)
Defines two commonly used APIs for Pisound - flash_leds and periodic_led_blink which are used by other scripts for feedback.