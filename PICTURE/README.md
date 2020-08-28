# Taking Screenshots and OCR quickly

## Dependencies

- `xclip`
- `imagemagick`
- a notification daemon (optional)

for `ocrshot`:

- `tesseract-ocr`, `tesseract-ocr-jpn` and `tesseract-ocr-jpn-vert` or any
other language pack for tesseract

## Installation

Copy the files to a directory in you `$PATH`, e.g. `/usr/local/bin/`.

```$ cp /path/to/PICTURES/{printscreen,ocrshot} /usr/local/bin/```

Change permissions.

```# chmod 755 /usr/local/bin{printscreen,ocrshot}```

## Usage

### printscreen

- `printscreen` takes a screenshot of a custom selected area with your mouse
- `printscreen -f` takes a screenshot of the currently focused window 
- `printscreen -r` takes a screenshot of the root window

By default the file is saved to your `/tmp` folder, but you may change the
variable `DIRECTORY` in the script.

The file's path is copied to the clipboard, so you can paste to a card in Anki.
Also, if you have a notification daemon running, you should get a popup message.

### ocrshot
![ocrshot in action](./ocrshot.gif)

Use the command without options to select an area on you screen and parse the
text with `tesseract`, an OCR engine. The default language is japanese vertical,
and you can call it with the `-H` option to read horizontal texts in japanese.
You should also get a notification saying whether text was detected or not.

If you want to use another language, make sure to install the appropriate
language support.

Remember that the OCR's ability to recognize text correctly depends on the
quality of the image. Even if you don't get it perfectly, it may still be close
enough and require a few corrections, which makes it good for mining scanned
manga, video games and subtitles as image.
