---
description: Solutions to most common issues.
---

# Troubleshooting

## Verify That Pisound Is Detected

Once the system boots up, open a terminal (command) window and run:
```
 amidi -l
 arecord -l
 aplay -l
```
You should see output similar to:
```
 pi@raspberrypi:~ $ amidi -l
 Dir Device    Name
 IO  hw:1,0    pisound MIDI PS-10JAD4Q
 pi@raspberrypi:~ $ arecord -l
 **** List of CAPTURE Hardware Devices ****
 card 1: pisound [pisound], device 0: PS-10JAD4Q snd-soc-dummy-dai-0 []
   Subdevices: 1/1
   Subdevice #0: subdevice #0
 pi@raspberrypi:~ $ aplay -l
 **** List of PLAYBACK Hardware Devices ****
 card 0: ALSA [bcm2835 ALSA], device 0: bcm2835 ALSA [bcm2835 ALSA]
   Subdevices: 8/8
   Subdevice #0: subdevice #0
   Subdevice #1: subdevice #1
   Subdevice #2: subdevice #2
   Subdevice #3: subdevice #3
   Subdevice #4: subdevice #4
   Subdevice #5: subdevice #5
   Subdevice #6: subdevice #6
   Subdevice #7: subdevice #7
 card 0: ALSA [bcm2835 ALSA], device 1: bcm2835 ALSA [bcm2835 IEC958/HDMI]
   Subdevices: 1/1
   Subdevice #0: subdevice #0
 card 1: pisound [pisound], device 0: PS-10JAD4Q snd-soc-dummy-dai-0 []
   Subdevices: 1/1
   Subdevice #0: subdevice #0
```

## Pisound Not Detected

If your Pisound is not detected, try these solutions:

1. **Make sure that the hardware connection is good.** Turn the system off and try to re-attach your Pisound to your Raspberry Pi. Boards should appear completely parallel, so you may have to squeeze the side with the pins a bit. You may try even without the spacers, just to make sure the connection is not the issue. 

2. **Check for system errors by [running `dmesg`](troubleshooting.md#running-dmesg).**


## Sound Quality Issues

If your Pisound has sound quality issues, try these solutions:

1. **Make sure the power supply is capable enough.** We recommend using a 5.1 V rated power supply, capable of providing 2.5 A of current or more. Read more [here](general-specifications.md#power-supply).

2. **Try different Jack settings.** Our recommended Jack settings can be found <a href="https://blokas.io/patchbox-os/docs/setup-wizard/" target="_blank">here</a>, but you may need to use higher values so feel free to experiment. If you are using the Patchbox OS, Jack parameters can be changed by running `patchbox` and choosing `jack` then the `config` option.

3. **Check for system errors by [running `dmesg`](troubleshooting.md#running-dmesg).**

## Other Issues

1. **If you are using the Patchbox OS, make sure you have the latest version.** To update the system, run `patchbox update`

2. **Check for system errors by [running `dmesg`](troubleshooting.md#running-dmesg).**

## Running `dmesg`

Once you get the `dmesg` output log, keep an eye out for lines containing **'pisound'** as well as any general errors, especially related to **'sound'**, **'snd'**, and **'device tree overlay'**. In case there's nothing obviously helpful in the log, try to post the entire log to our <a href="https://community.blokas.io/" target="_blank">community forum</a>.
