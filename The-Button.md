# The Button

The Button is a customizable button on the pisound board. There's 4 possible interactions with it:

1. Press, hold and release after 1 or more seconds passed. Executes `/usr/local/etc/pisound/hold.sh`
1. Click once. Executes `/usr/local/etc/pisound/single_click.sh`
1. Double click. Executes `/usr/local/etc/pisound/double_click.sh`
1. Triple click. Executes `/usr/local/etc/pisound/triple_click.sh`

Every **down** and **up** movement will also execute `/usr/local/etc/pisound/down.sh` and `/usr/local/etc/pisound/up.sh` respectively.

You may modify these scripts to do whatever you wish. Keep in mind that whenever you install / update the pisound-btn, if there were existing scripts in above locations, they are first backed up to `/usr/local/etc/pisound/backups/<current date>`, so if you had overridden the default scripts, you will have to restore your scripts manually.

**Note:** The pisound-btn daemon process must be running in order for the button to work. The 'make install' automatically adds it to the Desktop autostart.

## Safely Shutdown The OS
By default, holding the button down for more than 1 second and releasing will cause the system to shutdown cleanly by executing ```sudo shutdown now```. The MIDI activity LEDs will blink ten times quickly to confirm that the hold action was triggered. It is safe to unplug the power supply once RPi's activity LEDs stop flashing on and off. To restart, unplug and plug the power in again.

## Start a Pure Data Patch

By default, clicking the button once runs a script that scans the attached media storage devices for '**main.pd**', or, if not found in external media, scans `/usr/local/etc/pisound-patches/`, kills all existing instances of Pure Data, then starts a new instance opening the file, enables audio and connects all the detected MIDI inputs and outputs to the Pure Data's virtual MIDI ports. ALSA engine of Pure Data is used.

The script will blink the MIDI Activity LEDs once just after it reacted to the click, and again 2 times after it succeeded launching the patch, or one long duration blink if an error occurred or no patch was found.

## Turn Off & Eject USB

Double clicking will stop all Pure Data instances and unmount all attached external media, so it can be safely removed.

## Toggle WiFi Hotspot Mode

Triple-clicking will reconfigure the WiFi of RPi3 board or an external SoftAP capable USB WiFi adapter to behave as an Access Point (a.k.a. Wireless Router), as well as start 'touchosc2midi' monitor which will be ready to listen and forward MyOsc data as MIDI to other software such as Pure Data.

By default, the AP will appear as '**pisound**', and the default password is '**blokaslabs**' (without quotes). You can change the name and password by modifying `/usr/local/etc/pisound/hostapd.conf` Keep in mind that this file gets backed up and overwritten after reinstalling or updating the pisound-btn software.

The default IP address of RPi in WiFi Hotspot mode is 172.24.1.1 which you can use for ssh, VNC or wireless OSC / MIDI data! That means that you can easily interact with your system using just your laptop or phone, no more wires apart from power supply is needed! And it gets better, if the LAN cable is connected to RPi, it will share the Internet with the connected devices over WiFi!

To send MIDI OSC messages from your other devices to pisound, connect to the pisound's WiFi network, and set the 172.24.1.1 IP as the host in the software you're using (such as MyOSC or TouchOSC) settings. See [[here|faqs#how-to-send-wifi-midi-messages-to-your-raspberry-pi]] for more information.

To access RPi using ssh and/or VNC, make sure they're enabled in `raspi-config`. To enable, run in terminal:
```bash
sudo raspi-config
```
Go to **Interfacing Options** and make sure **ssh** and **VNC** are enabled. If you don't see these options, go to **Advanced Options** and do **Update**.

Triple-clicking again will revert to regular WiFi behavior.
