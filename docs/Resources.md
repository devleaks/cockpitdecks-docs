The following resources are common to all buttons.

# Colors

Color can either be expressed by their name, [[pillow color names.png|as defined in the Python PILLOW package]], or by a tuple of 3 or 4 values, representing red, green, blue, and optionally the transparency.

All values should be in the [0..255] range. For colors, 0 is black, 255 is pure color; for transparency, 0 is transparent, 255 is opaque.

```yaml
    icon-color: (100, 40, 40, 200)
    label-color: darkorange
    text-color: mediumaquamarine
```

# Number Formatting

Cockpitdecks uses the same convention as [python number to text formatting](https://docs.python.org/3/library/string.html#format-examples).

# Fonts

Font files available to all decks are in the `deckconfig/resources/fonts` folder.

Font files must be either Truetype fonts (TTF) or Opentype fonts (OTF).

The name of the file is used to designate the font.

Fonts are loaded on start up.

```yaml
	text-font: DIN Condensed Black.otf
```

Cockpitdecks comes with a few fonts appropriate for aeronautical use: Standard formal fonts like DIN, fancier fonts like LED fonts, icon fonts like font-awesome and weather-icon.

For Truetype font, it is not necessary to add the `.ttf` extension. For Opentype fonts it is necessary to add the `.otf` extension.

> [NOTE] About font files.
> Fonts placed in the resources/fonts folder are available for images on decks. They are not necessarily available as « web fonts » available in web decks.
> To be available directly, not through Cockpitdecks generated images, web decks fonts have to be made available in the asset folder and referenced in proper CSS files.

# Icons

Icon image files available to all decks need to be placed in the `deckconfig/resources/icons` folder.

Image files must be either JPEG images (JPG, JPEG) or Portable Network Graphic (PNG).

The name of the file is used to designate the icon.

Icons are loaded on start up and optionally cached.

Typical icon size should be (square) 128 to 256 pixels.

Internally, Cockpitdecks use 256 pixel icons. Icons are resized to the deck requested size upon display.

```yaml
	icon: OFF_WHITE_FRAMED
```

# Documentation

Documentation of the aircraft decks can be placed in the folder `deckconfig/docs` or in the folder  `deckconfig/resources` in a textual form (including markdown), or as PDF.

# Decks

A particular aircraft allows for definition of particular decks. The deck definition is a special Yaml file that describes the deck capabilities and, optionally, for web decks, the layout of the buttons on the deck.

Please refer to the particular section on [[Deck Internals#Deck Type|Deck Types]] for details.

# Yaml

All configuration files are Yaml formatted. Yaml is simple, structured, and readable.

Yaml is loaded and interpreted by the python Yaml parser. Users must be aware that some *keywords* are sometimes interpreted by some parser. To prevent this interpretation, keywords should be placed between quotes.

For example:

`variable: on`

will be interpreted as boolean value *true*.

`variable: "on"`

will be interpreted as string `on`.

The following keywords have been noticeably discovered:

on, off, true, false, yes, no (all mapped to Boolean values True or False).

Cockpitdecks uses a Yaml 1.2 compliant parser ([ruamel.yaml](https://sourceforge.net/projects/ruamel-yaml/)) that refuse those interpretation as describe in the Yaml 1.2 specifications.
