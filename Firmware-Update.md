# Just Friends: Firmware Update Procedure


## How it works

The update procedure is done by playing a sound-file into the RUN jack of Just Friends. You'll need to use an audio playback device (like a computer or smart phone) to send the appropriate bits. With a special combination of settings, Just Friends will startup in bootloader mode and be waiting for you to upload the knowledge.

*We use a modified version of Émilie Gillet's [stm-audio-bootloader](https://github.com/pichenettes/stm-audio-bootloader) on Just Friends.*

### Download

First you'll need to download the [latest firmware](https://github.com/whimsicalraps/Just-Friends/releases/latest). Make sure it's on the device you'll use for playback!

### Get Connected

* **Turn off your synthesizer**
* Set all knobs fully counter-clockwise
* Set switches to sound & cycle
* Connect a patch cable between the RUN jack & your playback device

### Prepare your playback

* Set your sample-rate output to 48kHz (automatic on smart phones)
* Turn up the volume! 3/4 to maximum should do it
* Turn off notifications / sound effects (Do not disturb / Airplane mode is best)

### Let's Go!

* Turn on your synthesizer
* 6N's light should be slowly pulsing (IDENTITY should be lit or dim, but static)
* Start playback of the audio file, IDENTITY should flash subtly, showing signal is being received
* 6N's light will move to 5N and dance with 4N for then next 90 seconds
* 3N will light temporarily and then Just Friends will restart anew!
* **Disconnect the cable from your playback device first, then from the RUN jack!**

All done!


## Shoot Troubles

Sometimes things just don't work out. Other times you can't take no for an answer. If the above instructions don't seem to be working correctly see if one of the below ideas might be your solution.

### 3N is lit as soon as you turn the synth on

This is probably a grounding issue. Connect / disconnect your playback sources power supply and then try again.

### 6N stays lit after audio starts

Sounds like the audio is either too loud, or too quiet.

If the IDENTITY light stays off, no signal is being understood by the module. Try increasing the volume on your playback device. If that doesn't work, check your patch cable is correctly connected, or perhaps turn off the synth & retry with a different cable.

If the IDENTITY light does come on, your audio is probably too loud. Just Friends is expecting a line-level signal, not a 10Vp-p Eurorack signal. If you're using a Eurorack preamp module for line-level signals, you'll need to turn the volume down below 1/4, maybe even less.

### 3N lights as soon as playback begins

This is likely a playback problem. Make sure your device is set to 48kHz playback sample rate. If you're using an audio DAW you probably have to do this in your DAW's settings. If the problem persists try a different playback device perhaps.

### Update begins ok but then 3N lights solidly

The audio has likely been interrupted. Perhaps you received a text message & it's sound alert corrupted the stream? Turn on Airplane mode and try again. If it happens at the same point in the file, redownload the file - it might be corrupted!
