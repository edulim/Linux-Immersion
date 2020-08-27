# Audio resources

Tools for recording audio and pasting the result into Anki, thus creating a
seamless workflow for card creation.

- `.asoundrc` is ALSA's user configuration file
- `record` is a script to start/stop recording playback audio, saving it as
  `.wav` and copying it to the clipboard
- `qolibry-play` is a script used to grab audio played within
  [qolibri](https://github.com/ludios/qolibri) that automatically copies it to
your clipboard

## Dependencies

- `xclip`

for `qolibri-play`:

- A media player.

for `record`:
- `ffmpeg` (optional; gives access to more file formats besides .wav)
- `sox` (optional; used to remove silence from recorded audio)
- A notification daemon (optional). 

## Installation

If you're using just ALSA and no Pulseaudio or JACK, copy `.asoundrc` to your
home directory.  

```$ cp /path/to/AUDIO/.asoundrc ~/```

[Edit
it](https://github.com/edulim/Linux-MIA/wiki/Audio-system-setup#setting-up-alsa),
replacing the capture and playback sound card with yours. The default card is
`hw:0,0`.

Change persmissions.

```$ chmod 755 /path/to/AUDIO/{record,qolibri-play}```

Copy the scripts `record` and `qolibri-play` to somewhere in your `$PATH`, e.g.
`/usr/local/bin/`.  

```$ cp /path/to/AUDIO/{record,qolibri-play} /usr/local/bin/```

Set `qolibri-play` as your external program for sound in qolibri. To do that, go
to `Settings > Options > External program`.

## Usage

### qolibri-play

Just play audio normally by clicking on a hyperlink and the audio file will be
copied to your clipboard. Paste with `Ctrl+C` within the Anki editor window or
`Ctrl + Shift + v` with the MIA Dictionary addon.

### record

Run the script `record` to start recording playback audio, and run it again to
stop recording. In case of running at an interactive shell with `ffmpeg`, one
may type `q` to stop recording instead of running the script a second time.
Paste with `Ctrl+C` within the Anki editor window or `Ctrl + Shift + v` with the
MIA Dictionary addon.

It's recommended to add a custom keybinding pointing to the script. Use your
window manager, desktop environment or a hotkey daemon, e.g.
[sxhkd](https://github.com/baskerville/sxhkd), for that.

## Caveats

1. If you're not using pure ALSA, using the file `.asoundrc` may not work. You
can always try playing with it, and delete it in case it breaks your current
setup. Read the
[wiki](https://github.com/edulim/Linux-MIA/wiki/Audio-system-setup) to
understand why that file may be necessary depending on you system and how to
setup the required kernel module.

2. `record` uses the PCM device called `looprec`, which is an output loopback
device defined in `.asoundrc`. If you aren't using that config file, you may
need to change the device used for recording loopback, depending on you custom
configuration.

3. After setting up, you may need to close and reopen your running programs in
order to use the new configuration file.

4. Unfortunately, the clipboard pasting feature has limited use to Anki and
other programs that work similarly.
