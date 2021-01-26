# Linux, Immersion and Language Acquisition
Resources for Linux users who are acquiring languages by immersion.

This repository provides files and instructions to replicate [a typical workflow
when mining cards](https://www.youtube.com/watch?v=CfvDKgNUSi8) in a Linux
system.

Using simple command line tools like `xclip`, `imagemagick`, `arecord` and
`ffmpeg`, it's possible to make scripts with dependencies that most users
should naturally fulfill. Each user is free to use and modify the scripts.

The folder structure is the following:

* AUDIO - how to record playback audio and export to Anki
    *  .asoundrc - config file for ALSA
    *  qolibri-play - script for grabbing audio from qolibri
    *  qolibri-mp3 - script for grabbing audio from qolibri as mp3
    *  record - script for recording playback audio
* PICTURE - how to quickly take screenshots and export them to Anki
    *  printscreen - script that takes a screenshot
    *  ocrshot - script that generates text in a target language from image

Most scripts are expected to run on X11, and ,for the time being, Wayland
users needs to adapt them by changing a few commands.

I encourage you to read the [wiki](https://github.com/edulim/Linux-MIA/wiki) 
prepared with basic information necessary to use the files provided in this
repo.

The wiki includes the following articles:

* [Audio system setup](https://github.com/edulim/Linux-MIA/wiki/Audio-system-setup)
* [Setting custom
  keybindings](https://github.com/edulim/Linux-MIA/wiki/Setting-custom-keybindings)
* [Copy subtitles with mpv](https://github.com/edulim/Linux-MIA/wiki/Copy-subtitles-with-mpv)
