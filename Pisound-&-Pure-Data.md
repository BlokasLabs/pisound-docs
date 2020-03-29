# Pisound & Pure Data

Using Pisound you can run your Pure Data patches straight from a USB thumb drive with a press of a button with no need to connect an external monitor, keyboard or mouse.

Plug in a USB drive containing your Pure Data patch, main patch file called **main.pd**, plug in any additional MIDI controller/keyboard, press The Button and Voilà!

You can read more about The Button functionality [here](The-Button.md).

Also you may be interested in the [Pisound App](Pisound-App.md).

If you haven't already, you can install Pure Data by running the following command in a Terminal window:

```
sudo apt-get install puredata
```

## Configuring Pure Data

By default, Pure Data is launched when [single-clicking](https://github.com/BlokasLabs/pisound/blob/master/scripts/common/start_puredata.sh#L60) the button as so:

```
puredata -stderr -alsa -audioadddev pisound -alsamidi -channels 2 -r 48000 -mididev 1 -send ";pd dsp 1" $@ &
```

You may want to customize the command line arguments for Pure Data according to your own needs. To do that, check the [Pure Data's Command Line](https://puredata.info/docs/faq/commandline) documentation and edit `/usr/local/pisound/scripts/common/start_puredata.sh`.
