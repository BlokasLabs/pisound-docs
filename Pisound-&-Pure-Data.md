# Pisound & Pure Data

Using Pisound you can run your Pure Data patches straight from a USB thumb drive with a press of a button with no need to connect an external monitor, keyboard or mouse.

Plug in a USB drive containing your Pure Data patch, main patch file called **main.pd**, plug in any additional MIDI controller/keyboard, press The Button and Voilà!

You can read more about The Button functionality [here](the-button).

Also you may be interested in the [[Pisound App]].

If you haven't already, you can install Pure Data by running the following command in a Terminal window:

```
sudo apt-get install puredata
```

## Configuring Pure Data

By default, Pure Data is launched when [single-clicking](https://github.com/BlokasLabs/pisound/blob/master/pisound-btn/single_click.sh#L88) the button as so:

```
nohup puredata -alsa -audioadddev pisound -alsamidi -channels 2 -r 48000 -mididev 1 -send ";pd dsp 1" "$PURE_DATA_PATCH" > /dev/null 2>&1 &
```

You may want to customize the command line arguments for Pure Data according to your own needs. To do that, check the [Pure Data's Command Line](https://puredata.info/docs/faq/commandline) documentation and edit /usr/local/etc/pisound/single-click.sh.

Keep in mind that updating Pisound's software will backup and overwrite your scripts, so in that case, you may want to re-apply your changes. See [here](software/#installing-the-driver) for more details.