# The Button

The Button is a customizable button on the Pisound board. There's a lot of mappable actions for it. The mappings are expressed in [`/etc/pisound.conf`](https://github.com/BlokasLabs/pisound/blob/master/pisound-btn/pisound.conf). You may use [`pisound-config`](pisound-config.md) to easily re-map the actions.

The `/etc/pisound.conf` file allows mapping these actions:

| Action Id | Description | Default script* |
| --- | --- | --- |
| DOWN | Button gets pushed down. | [down.sh](https://github.com/BlokasLabs/pisound/blob/master/scripts/pisound-btn/system/down.sh) |
| UP | Button gets released up. | [up.sh](https://github.com/BlokasLabs/pisound/blob/master/scripts/pisound-btn/system/up.sh) |
| CLICK_1 | Button was clicked once. | [start_puredata.sh](https://github.com/BlokasLabs/pisound/blob/master/scripts/pisound-btn/start_puredata.sh) |
| CLICK_2 | Button was double-clicked. | [stop_puredata.sh](https://github.com/BlokasLabs/pisound/blob/master/scripts/pisound-btn/stop_puredata.sh) |
| CLICK_3 | Button was triple-clicked. | [do_nothing.sh](https://github.com/BlokasLabs/pisound/blob/master/scripts/pisound-btn/do_nothing.sh) |
| CLICK_OTHER | Button was clicked between 4 and 8 times.  The upper limit is altered by the configuration file entry `CLICK_COUNT_LIMIT` or by the command line option [--click-count-limit](#command-line-options).  If the [--no-defaults](#command-line-options) options is specified and `CLICK_OTHER` is set the `CLICK_OTHER` script will run for all clicks that do not have specific entries in the configuration file. | [do_nothing.sh](https://github.com/BlokasLabs/pisound/blob/master/scripts/pisound-btn/do_nothing.sh) |
| CLICK_COUNT_LIMIT | Sets the click count limit.  Default is 8.  Can be overridden by the command line option [--click-count-limit](#command-line-options).  The absolute limit is 99. | No script action |
| HOLD_1S | Button was held down between 0.4s and 3s. | [toggle_bt_discoverable.sh](https://github.com/BlokasLabs/pisound/blob/master/scripts/pisound-btn/toggle_bt_discoverable.sh) |
| HOLD_3S | Button was held down between 3s and 5s. | [toggle_wifi_hotspot.sh](https://github.com/BlokasLabs/pisound/blob/master/scripts/pisound-btn/toggle_wifi_hotspot.sh) |
| HOLD_5S | Button was held down between 5s and 7s. | [shutdown.sh](https://github.com/BlokasLabs/pisound/blob/master/scripts/pisound-btn/shutdown.sh) |
| HOLD_OTHER | Button was held down for more than 7s. | [do_nothing.sh](https://github.com/BlokasLabs/pisound/blob/master/scripts/pisound-btn/do_nothing.sh) |

\* All default scripts are stored at `/usr/local/pisound/scripts/pisound-btn`

The hold action scripts get two arguments - first one for the count of clicks after which the button was held for a longer time and the second for the hold duration.  The hold duration is **always** an odd number.  It is calculated from the actual number of seconds as follows:  1  = zero to anything less than two,  3 = two to anything less than four, 5 = four to anything less than six,  and so on.  The absolute maximum number of seconds is 99.

The click action scripts get one argument - the number of clicks.

You may modify these scripts to do whatever you wish or re-map any of the action to execute your own custom script.

**Note:** The pisound-btn daemon process must be running in order for the button to work.

## Mapping the Actions

You may re-map the default actions or point them to your own custom scripts to be executed by modifying the [`/etc/pisound.conf`](https://github.com/BlokasLabs/pisound/blob/master/pisound-btn/pisound.conf).

Use [`pisound-config`](pisound-config.md) for effortless remapping.

The config file format allows placing comments by prefixing the line with # character. Comments must begin at the first word of the line.

Non-comment lines must consist of two space separated values. Spaces in the value are not permitted, therefore the absolute path to a custom script must not contain any spaces.

```
# Example comment
VARIABLE value_without_spaces
```

## Adding More Button Actions

For a new button action to appear in the [`pisound-config`](pisound-config.md) button action list, a new .sh script should be added to `/usr/local/pisound/scripts/pisound-btn/`. Make sure the 'execute' (`chmod +x ...`) permission is set for the new script. The file name will appear in `pisound-config`'s Pisound Button Settings which can be used for mapping as an action for The Button interaction.

The sky is definitely not the limit on what The Button can do, the shell scripts can be used to bootstrap things written in any other language.

### A Quick Tutorial

1. Run in a terminal the following commands:

		cd /usr/local/pisound/scripts/pisound-btn
		sudo touch hello_world.sh && sudo chmod +x hello_world.sh
		sudo nano hello_world.sh

2. Insert the following contents by copying and pasting (use **ctrl+shift+v** or **shift+insert** to paste into the terminal):

		#!/bin/sh
		. /usr/local/pisound/scripts/common/common.sh
		log "Hello World!"
		flash_leds 100

	Exit `nano` by hitting **ctrl+x**, press **y** and then **enter** to confirm write out.

3. Then run `pisound-config`:

		sudo pisound-config

	Enter **Change Pisound Button Settings** submenu, select **CLICK_1**, and pick the new **Hello World** in the list.

	Now effective immediately, pressing The Button once will do a long-flash of the MIDI LEDs, as well as log a timestamped *Hello World!* to The Buttons' log (`sudo journalctl -f -u pisound-btn`)

### What Did Just Happen?

In the first step, we have created a new script file with the execute permission set, and launched the `nano` editor.

In the second step, the script got coded. The `#!/bin/sh` indicates that this is a shell script. The `. /usr/.../common.sh` line imports the common functions defined in [`common.sh`Qhttps://github.com/BlokasLabs/pisound/blob/master/pisound-btn/scripts/common.sh). The `log` line prepends the timestamp to the given text to print and prints it to the system log. The `flash_leds` line interacts with the Pisound driver and causes the LEDs to flash. Try playing with the number argument to see the difference in flash duration. (Maximum value is 255)

In the third step we have remapped the single click action. This caused `/etc/pisound.conf` to get automatically updated accordingly.

## Main Action Scripts

### [`shutdown.sh`](https://github.com/BlokasLabs/pisound/blob/master/scripts/pisound-btn/shutdown.sh) - Safely Shutdown The OS
By default, holding the button down for more than 5 seconds and releasing will cause the system to shutdown cleanly by executing ```sudo shutdown now```. The MIDI activity LEDs will blink ten times quickly to confirm that the hold action was triggered. It is safe to unplug the power supply once RPi's activity LEDs stop flashing on and off. To restart, unplug and plug the power in again.

### [`start_puredata.sh`](https://github.com/BlokasLabs/pisound/blob/master/scripts/pisound-btn/start_puredata.sh) - Start a Pure Data Patch

If you haven't already, you can install Pure Data via [`pisound-config`](pisound-config.md), in the 'Install Additional Software' menu

By default, clicking the button once runs a script that scans the attached media storage devices for '**main.pd**', or, if not found in external media, scans `/usr/local/puredata-patches/`, kills all existing instances of Pure Data, then starts a new instance opening the file, enables audio and connects all the detected MIDI inputs and outputs to the Pure Data's virtual MIDI ports. ALSA engine of Pure Data is used.

The script will blink the MIDI Activity LEDs once just after it reacted to the click, and again 2 times after it succeeded launching the patch, or one long duration blink if an error occurred or no patch was found.

### [`stop_puredata.sh`](https://github.com/BlokasLabs/pisound/blob/master/scripts/pisound-btn/stop_puredata.sh) - Turn Off Pure Data & Eject USB

Double clicking will stop all Pure Data instances and unmount all attached external media, so it can be safely removed.

### [`toggle_wifi_hotspot.sh`](https://github.com/BlokasLabs/pisound/blob/master/scripts/pisound-btn/toggle_wifi_hotspot.sh) - Toggle WiFi Hotspot Mode

Pressing The Button for 3 seconds will reconfigure the WiFi of Raspberry Pi board (models with WiFi integrated) or an external SoftAP capable USB WiFi adapter to behave as an Access Point (a.k.a. Wireless Router), as well as start 'touchosc2midi' monitor which will be ready to listen and forward MyOsc data as MIDI to other software such as Pure Data.
TouchOSC2MIDI must be installed for this to work, you may get it installed using 'Install Additional Software' menu in [`pisound-config`](pisound-config.md).

By default, the AP will appear as '**Pisound**', and the default password is '**blokaslabs**' (without quotes). You can change the name and password by using [`pisound-config`](pisound-config.md).

The default IP address of RPi in WiFi Hotspot mode is 172.24.1.1 which you can use for ssh, VNC or wireless OSC / MIDI data! That means that you can easily interact with your system using just your laptop or phone, no more wires apart from power supply is needed! And it gets better, if the LAN cable is connected to RPi, it will share the Internet with the connected devices over WiFi!

To send MIDI OSC messages from your other devices to Pisound, connect to the Pisound's WiFi network, and set the 172.24.1.1 IP as the host in the software you're using (such as MyOSC or TouchOSC) settings. See [here](faqs.md#how-to-send-wifi-midi-messages-to-your-raspberry-pi) for more information.

To access RPi using ssh and/or VNC, make sure they're enabled in `raspi-config`. To enable, run in terminal:
```bash
sudo raspi-config
```
Go to **Interfacing Options** and make sure **ssh** and **VNC** are enabled. If you don't see these options, go to **Advanced Options** and do **Update**.

Triple-clicking again will revert to regular WiFi behavior.

### [`toggle_bt_discoverable.sh`](https://github.com/BlokasLabs/pisound/blob/master/scripts/pisound-btn/toggle_bt_discoverable.sh) - Toggle Bluetooth Discoverability On and Off

Holding The Button between 1 and 3 seconds will toggle the Bluetooth Discoverability. See [Pisound App](pisound-app.md) for controlling Pisound & RPi remotely.

## Miscellaneous Scripts

### [`do_nothing.sh`](https://github.com/BlokasLabs/pisound/blob/master/scripts/pisound-btn/do_nothing.sh)
A script which literally does no nothing and is very good at it.
### [`down.sh`](https://github.com/BlokasLabs/pisound/blob/master/scripts/pisound-btn/system/down.sh)
A script that gets triggered every time The Button is pushed down. By default it has code to start periodic LED flashing, in case The Button is going to remain held down.
### [`up.sh`](https://github.com/BlokasLabs/pisound/blob/master/scripts/pisound-btn/system/up.sh)
The counter part of `down.sh` - it gets triggered every time The Button is released and by default it is stopping the LED flashing initiated in the down event.
### [`common.sh`](https://github.com/BlokasLabs/pisound/blob/master/scripts/common/common.sh)
Defines APIs shared between scripts for Pisound, such as flash_leds and periodic_led_blink which are used by other scripts for feedback.

## Command Line Options

Current command line options are always available using the `--help` argument.  The options are:

| Option | Description |
| --- | --- |
|`--help`| Display the usage information.|
|`--version`| Show the version information. |
|`--gpio <n>`| The pin GPIO number to use for the button. Default is 17. |
|`--active-low`| Reverse the sense of the active state.  Normally active is when GPIO goes high. |
|`--conf <path>`| Specify the path to configuration file to use. Default is /etc/pisound.conf. |
| `--click-count-limit <n>` | Set the click count limit to n. Use 0 for no limit. Default is 8.  This option will override the `CLICK_COUNT_LIMIT` configuration file entry.  The absolute limit is 99. |
|`--no-defaults`| Do not use the default values for click and hold.  Only configuration options will be used. |
|`--debug <n>`| Enable debugging at level n (higher value = more logging). |
|`-n <n>`| Short for --click-count-limit. |
|`-q`| Short for --debug 0 (turns off all but errors). |

