# Audio resources

Tools for recording audio and pasting the result into Anki, thus creating a
seamless workflow for card creation.

- `.asoundrc` is ALSA's user configuration file
- `record` is a script to start/stop recording playback audio, saving it as
  `.wav` and copying it to the clipboard
- `qolibry-play` is a script used to grab audio played within
  [qolibri](https://github.com/ludios/qolibri) that automatically copies it to
your clipboard
- `qolibri-mp3` copies the file as `.mp3`

## Dependencies

- `xclip`

for `qolibri-play` and `qolibri-mp3`:
- `ffmpeg` (only for `qolibri-mp3`)
- A media player.

for `record`:
- `ffmpeg` (optional; gives access to more file formats besides .wav)
- `sox` (optional; used to remove silence from recorded audio)
- A notification daemon (optional). 

## Installation

If you're using just ALSA and no Pulseaudio or JACK, copy `.asoundrc` to your
home directory.  

```$ cp /path/to/AUDIO/.asoundrc ~/```

[Edit it](https://github.com/edulim/Linux-MIA/wiki/Audio-system-setup#setting-up-alsa),
replacing the capture and playback sound card with yours. The default card is
`hw:0,0`.

Change permissions.

```$ chmod 755 /path/to/AUDIO/{record,qolibri-play,qolibri-mp3}```

Copy the scripts `record` and `qolibri-play` to somewhere in your `$PATH`, e.g.
`/usr/local/bin/`.  

```$ cp /path/to/AUDIO/{record,qolibri-play,qolibri-mp3} /usr/local/bin/```

Set `qolibri-play` or `qolibri-mp3` as your external program for sound in qolibri. To do that, go
to `Settings > Options > External program`.

## Usage

### qolibri-play and qolibri-mp3

Just play audio normally by clicking on a hyperlink and the audio file will be
copied to your clipboard. Paste with `Ctrl+C` within the Anki editor window or
`Ctrl + Shift + v` with the MIA/Migaku Dictionary addon.

`qolibri-play` copies the raw file from qolibri, which is `.wav` file, but that
isn't supported by the current MIA/Migaku Dictionary addon Card Exporter. You may still
use the regular Anki editor window, though. Use `qolibri-mp3` if you want to
export to the addon window.

### record

The script should detect whether you're running Pulseaudio and use the
appropriate command for recording, i.e. `parec´ for Pulseaudio and `arecord` for
alsa. Also, the first lines of the script contain variables that can be manually
changed by the user, such as ´FORMAT´ (wav, flac, ogg, etc), `OUTPUT´,
´ALSA_RECORDING_DEVICE´ and `PULSEAUDIO_RECORDING_DEVICE`.

`ALSA_RECORDING_DEVICE` is already set to the value `looprec`, which is in
accordance with the `.asoundrc` file provided in this repository. However,
`PULSEAUDIO_RECORDING_DEVICE` should be changed depending on the output
of

```$ pactl list short sources```.

That should look like:
´´´
0	alsa_output.pci-0000_01_00.1.hdmi-stereo.monitor	module-alsa-card.c	s16le 2ch 44100Hz	SUSPENDED
1	alsa_output.pci-0000_00_14.2.analog-stereo.monitor	module-alsa-card.c	s16le 2ch 44100Hz	SUSPENDED
2	alsa_input.pci-0000_00_14.2.analog-stereo	module-alsa-card.c	s16le 2ch 44100Hz	SUSPENDED
3	alsa_output.platform-snd_aloop.0.analog-stereo.monitor	module-alsa-card.c	s16le 2ch 44100Hz	SUSPENDED
4	alsa_input.platform-snd_aloop.0.analog-stereo	module-alsa-card.c	s16le 2ch 44100Hz	SUSPENDED
```

Either the index or the name of the device may be used. For example, if one
wants to record audio from the output channel of the analog connection,
`PULSEAUDIO_RECORDING_DEVICE` may be set to 1 or `alsa_output.pci-0000_00_14.2.analog-stereo.monitor`.

'Run the script `record` to start recording
playback audio, and run it again to stop recording.  Paste with `Ctrl+C` within
the Anki editor window or `Ctrl +
Shift + v` with the MIA/Migaku Dictionary addon.

`record` will automatically stop recording after a few seconds defined by the
variable `MAX_RECORD_TIME`, in order to prevent very long files that might be
recorded by accident and without noticing. It's set to record up to 600 seconds
(10 minutes) by default.

It's recommended to add a custom keybinding pointing to the script. Use your
window manager, desktop environment or a hotkey daemon, e.g.
[sxhkd](https://github.com/baskerville/sxhkd), for that.

## Caveats

1. If you're not using pure ALSA, using the file `.asoundrc` may not work. You
can always try playing with it, and delete it in case it breaks your current
setup. Read the
[wiki](https://github.com/edulim/Linux-Immersion/wiki/Audio-system-setup) to
understand why that file may be necessary depending on you system and how to
setup the required kernel module.

2. `record` uses the PCM device called `looprec`, which is an output loopback
device defined in `~/.asoundrc`. If you aren't using that config file, you may
need to change the device used for recording loopback, depending on you custom
configuration.

3. After setting up, you may need to close and reopen your running programs in
order to use the new configuration file.

4. If the recorded audio volume is too low, you may apply a filter to the output
 audio. An example using ffmpeg would be:

```
ffmpeg -f alsa -i "$RECORDING_DEVICE" -filter:a "volume=3.0" "$OUTPUT"
```

4. Unfortunately, the clipboard pasting feature has limited use to Anki and
other programs that work similarly.

6. The MIA/Migaku Dictionary addon currently only supports 'mp3' files, but I've opened
   a pull request to add support for more containers. By default, the scripts
generate `.wav` files, so there are 3 possibilities to workaround.

-  Patch your custom addon using [my fork](https://github.com/edulim/MIA-Dictionary-Addon).
-  Use `record` with `ffmpeg` and set the file to `.mp3` in the script.
`qolibri-mp3` converts the standard `.wav` file to `.mp3` and copies it to the
clipboard
-  Don't use the Card Exporter feature and paste the clipboard's contents
directly to Anki's card editor window.
