---
description: A standard MIDI In/Out interface is available via two female DIN-5 connectors.
---

# MIDI

A standard MIDI interface is available via two female DIN-5 connectors. Unlike usual MIDI solutions for Raspberry Pi, MIDI on Pisound is implemented using high speed SPI and a dedicated microcontroller for translating SPI data to serial MIDI byte streams and it's readily recognized by audio software as an ALSA MIDI device. The loopback latency of MIDI was measured to be 2.105ms. In addition, there are MIDI activity LEDs for both Input and Output indicating the flow of MIDI events.

In addition, Pisound lets you take advantage of WiFi-MIDI. When WiFi Hotspot mode is enabled via [pressing The Button for 3 seconds](the-button.md#toggle_wifi_hotspotsh-toggle-wifi-hotspot-mode) (unless the WiFi toggle was remapped), `touchosc2midi` daemon gets launched. It translates OSC messages to MIDI events so you can control audio software running on Raspberry Pi from your smartphone or tablet.
TouchOSC2MIDI must be installed for this to work, you may get it installed using 'Install Additional Software' menu in [`pisound-config`](pisound-config.md).

And of course you can use USB-MIDI devices as usual by connecting them to Raspberry Pi USB ports.

![signal delay between MIDI](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/midi_latency.png)

An oscillogram showing signal delay between MIDI input (yellow) and MIDI output (cyan) of 2.105 ms when echoing
