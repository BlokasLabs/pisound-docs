# Pisound App
![pisound-app](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/pisound-app.jpg)

The mobile app communicates with a dedicated server running on Raspberry Pi & Pisound via Bluetooth. It is called Pisound Control Server (`pisound-ctl`) and it allows listing all supported patches in predefined locations (usually `/usr/local/...-patches`) and to selectively launch the patches in the appropriate software, making headless browsing and switching between patches easy, as well as executing custom scripts.

The stdout and stderr outputs of the launched application are displayed in real time, informing the user on what is going on with the patch application.

The design is extensible - new scripts may be added to support launching patches on more audio applications, see the [Customization](#customization) section for more details.

## Software Setup
### Raspberry Pi

First, make sure that Pisound is set up and that `pisound-btn --version` says it’s 1.05 or higher and `pisound-ctl --version` says it's 1.02 or higher. If it is not, follow the install instructions on [Installing/Updating The Pisound Software](software#installingupdating-the-pisound-software). The `install-pisound.sh` will update the software even if some previous version was already installed.

If using Raspberry Pi without built-in Bluetooth support, connect a USB Bluetooth dongle to it.

After everything is set up, `pisound-ctl` will get launched automatically and will be added to system auto start.

### Android

Install the Pisound app on your device [here](https://play.google.com/store/apps/details?id=com.blokas.pisoundctl), or download the apk directly [here](https://blokas.io/pisound/app/com.blokas.pisoundctl.v1.02.apk).

#### Why is the Location Permission Required?
As much as we hate excess permission requests ourselves, there’s no way we can avoid requesting this one. We don’t need or store your location information. It’s required for an application to declare [ACCESS_COARSE_LOCATION](https://developer.android.com/reference/android/Manifest.permission.html#ACCESS_COARSE_LOCATION) permission in its Manifest for the application to be able to initiate discovery of nearby Bluetooth devices.

This is a new requirement since Android OS 6.0 - a nice summary of the issue is available [here](https://stackoverflow.com/questions/33045581/location-needs-to-be-enabled-for-bluetooth-low-energy-scanning-on-android-6-0). And here's some [more info](https://developer.android.com/about/versions/marshmallow/android-6.0-changes.html#behavior-notifications) on the new behavior.

### iOS

Coming soon...

## Using the App
### Connecting to the Raspberry Pi

Hold The Button on Pisound for 3 LED blinks (but less than 5 or the shutdown will get triggered instead) to make it discoverable to other Bluetooth devices. After a while (180 seconds by default), it will automatically switch off the discoverability and the LED blinking should stop. This needs to be done only for initially pairing your device to Raspberry Pi. Afterwards, if using same devices and if they weren’t unpaired, switching discoverability on is not needed, you may simply connect to the last used device. You may manually turn the discoverability off before it times out on Raspberry Pi by holding The Button for 3 LED blinks.

To connect to a new device, click 'Scan' in the initial screen and wait for nearby devices to be discovered. Once you see your Raspberry Pi in the list (usually `raspberrypi` unless you changed its hostname), tap it to connect to it. In case it didn’t appear in the list, make sure that it is in discoverable mode and you may pull the list down or click the button that appears at the bottom in order to manually initiate a new scan of nearby devices.

If when starting the app it was already successfully connected to some device before, it would attempt to automatically connect to that device once.

### MIDI Controllers

At the moment a patch gets launched, the app connects all of the connected MIDI devices to the patch host, so you may immediately interact with the system.

The built-in patches are organized in 8 MIDI CC's per page of controls. The numbering of CCs starts from 2 and CC 33 is skipped. That is because CC 0 is not usable  on some MIDI controllers and CC 1 and CC 33 messages are dedicated for the Modulation Wheel.

The patches that have a MIDI parameter listing have a 'MIDI' button at the top right of the patch view which opens the full list of the controllable patch parameters.

We suggest programming your MIDI controller's controls in the same way as described above, but you may map it according to the MIDI parameter list to your convenience.

### Starting a Patch

The app comes bundled with a set of great Pure Data patches to get you started, as well as some convenience System Scripts.

Go to the 'Collections' tab at the bottom to retrieve the categories of items. Click on Pure Data to get a list of patches, pick one of the patches and hit the start button at the bottom right to launch it.

The launched patch then can be reached in the 'Active' tab, and its console output is available in the 'Console' tab.

#### Pure Data

The Pure Data patches are expected to be found in `/usr/local/puredata-patches/`. Each patch should be in its own subfolder. The legacy patches require the entry point into the patch to be named 'main.pd'. The new way of including patches is to create a '[blokas.yml](#blokasyml)' file in the patch folder which stores various pieces of information about the patch, including the name of its entry point.

### Deleting a Patch

You may choose to delete the patch by clicking the 'Delete' button in the patch description view. Be careful, there's no way to easily undo the action!

### Importing Patches from USB

Clicking 'Import patches from USB' in the Home tab will run the `/usr/local/pisound-ctl/usb_import.sh` script. It scans the connected USB media for folders like 'puredata-patches' in the media root, and imports it to the collection on the sd card (`/usr/local/...-patches`).

While import is ongoing, the app will display a console. Lines that say 'Importing ...' means a brand new patch was copied over while 'Merging ...' lines mean the patch was already in the collection and that it was overwritten.

### Managing the Server

To stop the server from automatically starting on boot:

    sudo systemctl disable pisound-ctl
 

To re-enable automatic start:

    sudo systemctl enable pisound-ctl


## Customization

The main customizable parts are the scripts and .yml files in `/usr/local/pisound-ctl/`.

### [category_list.sh](https://github.com/BlokasLabs/pisound-ctl-scripts/blob/master/category_list.sh)

This script is responsible for producing the list of categories supported in the app. The produced output should be paths to .yml files which describe the category.

You may add more kinds of collections by modifying this file and creating appropriate .yml file.

### [category.yml](https://github.com/BlokasLabs/pisound-ctl-scripts/blob/master/puredata/puredata.yml)

The category yml file describes the category of items as well as provides a couple of scripts which are used to implement particular functions.

### [list.sh](https://github.com/BlokasLabs/pisound-ctl-scripts/blob/master/puredata/puredata_list.sh)

This script should print the paths to the patch entry points or metadata files (usually the path to blokas.yml), so they appear in the collection view.

### [blokas.yml](https://github.com/BlokasLabs/pisound-ctl-scripts/blob/master/system/restart/blokas.yml)

The patch yml file describes the patch itself and contains info that gets displayed in the 'Patch View' of the app.

### [detail.sh](https://github.com/BlokasLabs/pisound-ctl-scripts/blob/master/puredata/puredata_detail.sh)

A script that outputs the metadata on the patch - usually it's enough to print the blokas.yml file for the patch, however, some more advanced uses may generate the data on the fly. YAML or equivalent JSON output is acceptable.

At the very least, 'entry' and 'name' must be produced.

### [launch.sh](https://github.com/BlokasLabs/pisound-ctl-scripts/blob/master/puredata/puredata_launch.sh)

A launcher script for executing the item. It gets the item path in its first argument, and whatever was in 'args' attribute in the rest of the command line arguments.

### [stop.sh](https://github.com/BlokasLabs/pisound-ctl-scripts/blob/master/puredata/puredata_stop.sh)

A stopper script - executed when the user stops the patch. If possible, it may gracefully cause the patch host to exit or stop the patch. If it doesn't, the patch host gets killed. (which is fine in most cases)

## Contribution Guide

The scripts used by `pisound-ctl` are hosted on GitHub here: https://github.com/BlokasLabs/pisound-ctl-scripts.

You may submit pull requests with your modifications and additions.
