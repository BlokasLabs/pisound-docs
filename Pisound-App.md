# Pisound App

The mobile app communicates with a dedicated server running on Raspberry Pi & Pisound via Bluetooth. It is called Pisound Control Server (`pisound-ctl`) and it allows listing all supported patches in predefined locations (`/usr/local/etc/...-patches`) and to selectively launch the patches in the appropriate software, making headless browsing and switching between patches easy.

The stdout and stderr outputs of the launched application are displayed in real time, informing the user on what is going on with the patch application.

The design is extensible - new scripts could be added to support launching patches on other audio applications, based on the file extension, see the [Customization](#customization) section for more details.

## Software Setup
### Raspberry Pi

First, make sure that Pisound is set up and that `pisound-btn --version` says it’s 1.04 or higher. If it is not, follow the install instructions on [Installing the Driver](software#installing-the-driver). The `install-pisound.sh` will update `pisound-btn` if it is already installed.

If using Raspberry Pi without built-in Bluetooth support, connect a USB Bluetooth dongle to it.

Then, add Blokas’ apt server:

    wget -q -O - https://blokas.io/apt-setup.sh | sh

And install the Pisound Control Server:

    sudo apt-get install pisound-ctl

After installing, `pisound-ctl` will get launched automatically and will be added to system auto start.

### Android

Accept beta testing invitation [here](https://play.google.com/apps/testing/com.blokas.pisoundctl). (Make sure you’re doing this logged in with the account you’re using on Play Store)

The beta acceptance propagation in Google servers seems to take a while, the Play Store link below **will take up to 20 minutes** before it starts working for you. Until then, it will show you “_We’re sorry, the requested URL was not found on this server._” We know what you’re thinking, but no - the link is fine, just be patient and hit F5. 🙂 It may take even longer for the app to appear when searching for ‘Pisound’ on mobile version of the Play Store, however, you may request for the app to be installed remotely to your device using the link below.

Install the Pisound app on your device [here](https://play.google.com/store/apps/details?id=com.blokas.pisoundctl).

#### Why is the Location Permission Required?
As much as we hate excess permission requests ourselves, there’s no way we can avoid requesting this one. We don’t need or store your location information. It’s required for an application to declare [ACCESS_COARSE_LOCATION](https://developer.android.com/reference/android/Manifest.permission.html#ACCESS_COARSE_LOCATION) permission in its Manifest for the application to be able to initiate discovery of nearby Bluetooth devices.

This is a new requirement since Android OS 6.0 - a nice summary of the issue is available [here](https://stackoverflow.com/questions/33045581/location-needs-to-be-enabled-for-bluetooth-low-energy-scanning-on-android-6-0).

[More info](https://developer.android.com/about/versions/marshmallow/android-6.0-changes.html#behavior-notifications) on the new behavior.

## Using the App
### Connecting to the Raspberry Pi

Hold The Button on Pisound for 3 LED blinks (but less than 5 or the shutdown will get triggered instead) to make it discoverable to other Bluetooth devices. After a while (180 seconds by default), it will automatically switch off the discoverability and the LED blinking should stop. This needs to be done only for initially pairing your device to Raspberry Pi. Afterwards, if using same devices and if they weren’t unpaired, switching discoverability on is not needed, you may connect as usual. You may manually turn the discoverability off before it times out on Raspberry Pi by holding The Button for 3 LED blinks.

The initial screen of the Pisound app will list the paired Bluetooth devices immediately, and will automatically initiate discovery of other nearby devices. Once you see your Raspberry Pi in the list (usually `raspberrypi` unless you changed its hostname), tap it to connect to it. In case it didn’t appear in the list, make sure that it is in discoverable mode and you may pull the list down in order to manually initiate a new scan of nearby devices.

If when starting the app it was already successfully connected to some device before, it would attempt to automatically connect to that device once.

### Starting a Patch

Click ‘Start a Patch’ button - it will display a list of patches produced by `/usr/local/etc/pisound-ctl/patch_list.sh` script on Raspberry Pi. You may modify it to fit your needs or add support for other software.

After you click on one of the list items, `/usr/local/etc/pisound-ctl/patch_run.sh <patch>` will get executed. This script is responsible for launching the software to handle the selected patch. First it extracts the extension, and dispatches handling to a script dedicated for the given extension. In Pure Data patch case, it detects ‘.pd’ extension, and executes `/usr/local/etc/pisound-ctl/run_pd.sh <patch>`.

Feel free to modify the scripts, add support for other software, etc… Contributions are welcome! 🙂 See [Contribution Guide](#contribution-guide) for details on how you can participate.

After the patch is launched, the app immediately switches to the Output view page.

You may pull the list down to refresh it.

Pressing the ‘back’ button would go back to the main menu.

#### Pure Data

The Pure Data patches may consist of multiple .pd files. The convention is to have a subfolder for the entire patch, and the entry point to it should be called 'main.pd', and only files named like that are being searched for. An example location would be `/usr/local/etc/puredata-patches/testtone/main.pd`

### Looking at Output

The ‘Show Output…’ button initially is disabled, until there is some output to show. If it’s enabled, pressing the button would navigate to the Output display page. If the bottom line is visible in the view, the view will auto-scroll to keep showing the very latest lines. In case you manually scroll up, it will remain in that position. If you manually scroll down to the bottom again, it would resume auto-scrolling.

The lines produced by stdout are colored black and the stderr lines are colored red.

### Disconnecting

Clicking the ‘Disconnect' button in the main menu disconnects from the current device and goes back to the devices list / connect page.

### Managing the Server

To stop the server from automatically starting on boot:

    sudo systemctl disable pisound-ctl
 

To re-enable automatic start:

    sudo systemctl enable pisound-ctl


## Customization

The main customizable parts are the scripts in `/usr/local/etc/pisound-ctl/`.

### [`patch_list.sh`](https://github.com/BlokasLabs/pisound-ctl-scripts/blob/master/patch_list.sh)

This script is responsible for producing the list of items to show in ‘Start a Patch’ menu. Every line produced to stdout will appear as an item in the patch list.

You may extend this file to support more kinds of files by getting their filenames to be echoed.

### [`patch_run.sh`](https://github.com/BlokasLabs/pisound-ctl-scripts/blob/master/patch_run.sh)

This script receives the patch to launch in its arguments. It is responsible for checking the type of the provided patch and executing the appropriate launcher script. The last command of the script must launch the actual application which will handle the patch in blocking manner. Any post-setup which may be necessary (such as `aconnect` for MIDI) must be scheduled in background with a delay so it happens after the main app gets started. It must done this way so that the `pisound-ctl` is able to terminate the patch by killing the process id of the script.

You may extend this file for handling more kinds of patches by extending the case statement.

### [`run_pd.sh`](https://github.com/BlokasLabs/pisound-ctl-scripts/blob/master/run_pd.sh)

A launcher script for executing Pure Data. It has a block for the delayed post-setup of the software MIDI connections to the Pure Data. You may tweak the command line arguments which are used to launch Pure Data if necessary.

You should create a dedicated script for launching a new file extension in its host software and get it called from `patch_run.sh`.

## Contribution Guide

The scripts used by `pisound-ctl` are hosted on GitHub here: https://github.com/BlokasLabs/pisound-ctl-scripts.

You may submit pull requests with your modifications and additions.