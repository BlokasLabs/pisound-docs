# Audio

pisound is equipped with 192kHz 24-bit Stereo Input and Output. The legendary Burr-Brown Op-Amps, ADC and DAC chips are implemented in this little board to exhibit clean signal amplification and conversion between digital and analog realms. Though these chips themselves have a good power supply rejection ratio (PSRR), to ensure low-noise operation in the vicinity of an electrically noisy Raspberry Pi computer, any power line to the analog stuff is coupled via LDO’s which filters any digital interference out. This design enables pisound to be graded as a high-fidelity audio device. For example a Hi-Fi device is expected to have the total harmonic distortion value (THD) less than 1% and the pisound has less than 0.05%. That means that you can daisy-chain 20 pisound boards and the chain would still be considered as a high-fidelity device!

## Audio Input
There is one unbalanced stereo input accessible via 1/4" (6.35mm) jack slot on pisound. One stereo channel can also be used as two unbalanced mono channels. Audio inputs are AC coupled to a gain stage built using [OPA4134](http://www.ti.com/lit/ds/symlink/opa2134.pdf) op-amps. Input resistance is 100kOhm for each channel. The gain can be adjusted simultaneously for both the left and the right channels from 0dB to +40dB with an on-board potentiometer. The maximum audio signal level before clipping is 5Vpp (at 0dB gain). The range of the gain adjustment can be divided into two sections. The first section occurs at rotation between 0% and 80% and it is used to precisely adjust for the high-level signals (line out, headphone amp out, etc...). The tight section at the maximum rotation of the gain pot acts as a +20dB switch for the low-level signals (guitar, microphone, etc.). When the signal clipping occurs in any channel, the red LED lights up and fades out after last clipped sample. Audio to digital conversion is carried out by [PCM1804](http://www.ti.com/lit/ds/symlink/pcm1804.pdf) converter. An on-board clock oscillator delivers a clock signal to the ADC, which divides it according to the selected sample rate. ADC acts as the master of I2S line. pisound supports three sample rates: 48kHz, 96kHz and 192kHz. A filter at the input stage of PCM1804 ensures good anti-[aliasing](https://en.wikipedia.org/wiki/Aliasing).

![pisound-side](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/gain_vs_pot.jpg)
Gain and input sensitivity versus position of **GAIN** pot

## Audio Output
Audio output is DC coupled and can be accessed via the female 1/4" (6.35mm) stereo jack connector. Output volume level can be adjusted with on-board potentiometer. Maximum output level is 2.1Vrms when driving 1kOhm load. Digital to audio conversion is done in [PCM5102A](http://www.ti.com/lit/ds/symlink/pcm5100a-q1.pdf) converter which has both Signal-to-Noise ratio and dynamic range of 112dB. The DAC acts as a slave on I2S line and the sampling rate is dictated by the ADC. An intelligent muting system is used to prevent pops and clicks when disconnecting power supply.

## Audio Latency and Other Parameters

In digital audio equipment it takes time for the signal at input to be processed and delivered at output. This time is called audio latency. There's three parts to it. The first is the time required for the ADC to do the conversion and to send digital data to the processing unit. The second step is to process data and prepare it for the transfer to the DAC. And the final part is getting the data to the DAC and converting it to the analog signal. The most time consuming is the second part. Parts one and three often require no more than 1 ms depending on architecture of ADC and DAC, sample rate and in-built digital filters.

![audio latency of 2.092 ms](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/192kHz_audio_latency.png)

An oscillogram showing audio latency of 2.092 ms. pisound and Raspberry Pi 2 working at sample rate of 192 kHz

![spectrum of looped white noise](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/48kHz_BW.jpg)
Figure 1  Response of the sine signal fed to loop-backed (digital→DAC→ADC→digital) pisound showing the Signal-to-Noise Ratio (SNR) of 110 dB. Calculated Total Harmonic Distortion (THD) is less than 0.05%.

![spectrum of looped 1 kHz sine signal](https://raw.githubusercontent.com/wiki/BlokasLabs/pisound-docs/images/48kHz_THD.jpg)
Figure 2 Frequency response of the white noise fed to loop-backed (digital→DAC→ADC→digital) pisound showing the bandwidth (BW) of the device and how it is estimated.
