# MODEP - MOD Emulator for Pisound
![modep-intro](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/modep-intro.PNG)

## Introduction

MOD or Musician Operated Device is “a multi-effects pedal that pushes the limit of your guitar, bass, keyboard or any other instrument to craft the sounds you want and reproduce them instantly at home, studio or on stage.”

MOD is based on Linux SBC and LV2 plugins ecosystem. It has an intuitive drag-and-drop web-based interface so you can assemble your pedalboards as you do in real life. You can find more information about MOD workflow [here](https://moddevices.com/pages/mod-duo).

As it should be clear for you by now, MODEP is an emulator that will allow you to play around with MOD system using your Raspberry Pi and Pisound board.

By the way, MOD is not only software, it’s also a nice piece of hardware and the MOD team has done a remarkable job for the whole Linux audio open-source community, so if you like this emulator you should get the real thing @ https://moddevices.com.

## Setup Instructions
Download a prebuilt OS image from [here](https://community.blokas.io/t/release-modep-2018-04-03/496) and follow [these instructions](https://www.raspberrypi.org/documentation/installation/installing-images/) to install the image on your SD card.

Default user details:

**Login: modep**  
**Password: blokaslabs**

## Running MODEP
- Mount your freshly baked SD card to your Pi.
- Power up your Raspberry Pi.
- Connect to it via ‘Pisound' Wi-Fi hotspot using your computer or tablet (psw:blokaslabs).
- Open your browser and go to this address http://172.24.1.1 (you should see a similar view to the image below).
- That’s it. Now you can start building your pedalboards.

Note that Jack is running as 'root' user, in case you'd like to manually run jack_capture or other jack utils, it should be done as 'root' too.

![modep-default](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/modep-default.png)

## Configuration

### Connecting to the Device via ssh

- `ssh modep@172.24.1.1` (if connected to Pisound WiFi, otherwise see [below](#quick-and-dirty-debug) to find the RPi's IP to use in place of 172.24.1.1)
- Enter the password: blokaslabs

### Editing Configuration Files

Here's a quick guide on how to modify system configuration files, such as `jack.service`. Once connected via `ssh`, use a text editor to edit the file, the most popular ones are covered here:

- **nano**: A simple text editor with familiar user interface to GUI based notepads.
    1. `sudo nano /usr/lib/systemd/system/jack.service`
    1. Make your changes using cursor keys to navigate.
    1. Hit Ctrl+X to close, press 'Y' to confirm saving your changes.  
        <br/>
- **vi**: A modal commands based text editor with rich features.
    1. `sudo vi /usr/lib/systemd/system/jack.service`
    1. Navigate to the place you want to change using h, j, k, l keys.
    1. Enter 'insert mode' by pressing i key.
    1. Make your changes.
    1. Hit Esc key to go back to command mode.
    1. Type :wq to write the changes and quit the editor.

### Alternative jack Settings

There's many different possible layouts of effects possible with very different processing requirements from one another, so tradeoffs between stability and latency have to be made when defining the default audio settings.
If you'd like to try and achieve a lower latency or more stability (less XRUNs) for your use case, you may try one of alternative configurations out, see [here](#editing-configuration-files) for editing instructions.

In a nutshell, the lower the audio buffer (based on -n and -p arguments), the lower is the latency, but also the lower is the stability. Same goes with the higher is the sampling rate (-r, acceptable values are 48000, 96000, 192000).
So you have to balance these values to best accommodate your use case.

For more details on the arguments, see Jack's [documentation](https://github.com/jackaudio/jackaudio.github.com/wiki/jackd(1)).

The line to edit in `jack.service` is [this one](https://github.com/BlokasLabs/modep-gen/blob/modep/stage3/03-install-mod/files/jack.service#L9):

```
ExecStart=/usr/bin/jackd -v -t 2000 -P 75 -d alsa -d hw:pisound -r 48000 -p 128 -n 2 -X seq -s -S
```

Try replacing with one of the below:

```
ExecStart=/usr/local/bin/jackd -v -t 2000 -s -d alsa -d hw:pisound -r 96000 -p 256 -n 3 -X seq
```
If you happen to find a good command line to use, let us know via e-mail, support chat or in our community!

Once the config is changed, you must either restart the system or at least the MODEP's services:

```
sudo systemctl stop mod-ui mod-host jack
sudo systemctl daemon-reload
sudo systemctl start jack mod-host mod-ui
```

## The Button

Here is the list of functions you can achieve using Pisound’s button.

|**Interaction**|**Action**|
|:-----|:-----|
| Click betwen 1 and 8 times | Load the pedalboard from the first bank on your list at index corresponding to the number of clicks (see the image below).|
| Hold between 3 and 5 seconds  | Turn Wi-Fi hotspot mode on/off.|
| Hold between 5 and 7 seconds | Turn your Raspberry Pi off.|

![modep-banks](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/modep-banks.PNG)

## Plugins

At the moment MODEP image comes with 294 LV2 plugins ranging from synths to heavy guitar distortion plugins.

## Quick and Dirty Debug Tricks
### Determining the IP Address of Raspberry Pi
1. Method 1
    1. If your Pi is connected to your local network via a network cable:
    1. Login into your local router via its webgui (usually something like http://192.168.1.254)
    1. Go to network map or network overview.
    1. Look up a device called raspberrypi or modep.
    1. The IP should be listed somewhere there.

1. Method 2
    - Connect a screen and a keyboard to your Pi.
    - You should see a text-based terminal (the linux cli).
    - Login in with user 'modep', password 'blokaslabs'.
    - Type `ifconfig -a` to show all network interface configurations.
    - If connected via WiFi look up the entry for "wlan0" field "inet".
    - If connected via ethernet cable look up the entry for "eth0" field "inet".
    - This is the IP-Address of your device.

### Test Connectivity Between Pi and PC/Laptop
1. In a terminal window (`cmd` on Windows), run `ping <IP-Address>`
1. If connectivity is good, you should see response times getting logged.
    1. On Windows, by default 4 ping requests are made and the utility exits.
    1. On other OSes, by default you have to manually quit by hitting Ctrl+C.

### Web-GUI Is Reachable, but No Sound Or "Add Device Error" happens

First of all, check input and output cables, volume and gain level.

If this fails, then try restarting the MODEP software:

1. Login via `ssh`.
1. Run:
    
    ```
    sudo systemctl stop mod-ui mod-host jack
    sudo systemctl start jack mod-host mod-ui
    ```
    
    After the services are restarted, the browser should reload automatically.

1. Check status of the services:

    ```
    sudo systemctl status jack
    sudo systemctl status mod-host
    sudo systemctl status mod-ui
    ```

    The status should be **active (running)**. If it's not, try checking the service logs for any errors:

    ```
    sudo journalctl -u jack
    sudo journalctl -u mod-host
    sudo journalctl -u mod-ui
    ```

### Distorted Sound
1. First check if Clip LED is lit up while audio input signal is playing.
1. If it is:
    1. Turn down 'GAIN' knob until the Clip LED no longer gets turned on. If the 'GAIN' is at minimum, turn down the output volume of the device connected to Pisound's Audio Input.
1. If you still get distorted sound after above is resolved:
    1. Check XRUNS-Status at the lower right corner of your browser window.
1. Try experimenting with [jack](#alternative-jack-settings) settings defined in `jack.service`, in particular the -n, -p, -r values. See [here](#editing-configuration-files) for instructions on how to edit it.
