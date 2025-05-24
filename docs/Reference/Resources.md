# Resources

## Fonts

Font files available to all decks are in the `deckconfig/resources/fonts` folder.

Font files must be either Truetype fonts (TTF) or Opentype fonts (OTF).

The name of the file is used to designate the font.

Fonts are loaded on start up.

```yaml
	text-font: DIN Condensed Black.otf
```

Cockpitdecks comes with a few fonts appropriate for aeronautical use: Standard formal fonts like DIN, fancier fonts like LED fonts, icon fonts like font-awesome and weather-icon.

For Truetype font, it is not necessary to add the `.ttf` extension. For Opentype fonts it is necessary to add the `.otf` extension.

> [!NOTE] About font files.
> Fonts placed in the resources/fonts folder are available for images on decks. They are not necessarily available as « web fonts » available in web decks.
> To be available directly, not through Cockpitdecks generated images, web decks fonts have to be made available in the asset folder and referenced in proper CSS files.

Cockpitdecks provides the following licensed fonts in its distribution:

- D-DIN (4 variants),
- Overpass, especially Overpass Mono,
- Segment7Standard,
- Split flaps Skyfont and SplitFlap-TV,
- Icon fonts Font Awesome and weathericons.

The following fonts are recommended:

- B612
- DSEG: Original 7-segment and 14-segment fonts

Any Truetype or OpenType font can be added to Cockpitdecks.

## Icons

Icon image files available to all decks need to be placed in the `deckconfig/resources/icons` folder.

Image files must be either JPEG images (JPG, JPEG) or Portable Network Graphic (PNG).

The name of the file is used to designate the icon.

Icons are loaded on start up and optionally cached.

Typical icon size should be (square) 128 to 256 pixels.

Internally, Cockpitdecks use 256 pixel icons. Icons are resized to the deck requested size upon display.

```yaml
	icon: OFF_WHITE_FRAMED
```

## Sounds

Sounds files can be provided very much like icons. Valid sound file formats are `wav` and `mp3`. Other sound file format may be added in the future, provided they can be played in a browser.

# Custom Decks

## Structure

### Deck Type

### Web Deck Background Image

# Observables

[[Observables]]

# Additional Configuration Attributes

`config.yaml`

## Default Values

## Number Formatting

Cockpitdecks uses the same convention as [python number to text formatting](https://docs.python.org/3/library/string.html#format-examples).

## Named Colors

Named colors are defined at the highest aircraft level. They behave like a placeholder for a color, an alternate name.

### Definition

In the main configuration file of the aircraft:

```
named-colors:
	COCKPIT_BACKGROUND: skyblue
	COCKPIT_NIGHT: slateblue
	HIGHLIGHT: coral
```

### Usage

In a button definition attributes:

```
label-color: HIGHLIGHT
```

## Colors

Color can either be expressed by their name, [[pillow color names.png|as defined in the Python PILLOW package]], or by a tuple of 3 or 4 values, representing red, green, blue, and optionally the transparency.

All values should be in the [0..255] range. For colors, 0 is black, 255 is pure color; for transparency, 0 is transparent, 255 is opaque.

```yaml
    icon-color: (100, 40, 40, 200)
    label-color: darkorange
    text-color: mediumaquamarine
```

## Theme

Rendering themes is a features that allows a Cockpitdecks developer to provide different sets of default values for colors, fonts, and textures.

### Theme Declaration

```
cockpit-theme: night
```

Theme default value is an empty string (no theme).

If a theme is used, its name is prepended to default attribute name when they are looked up:

```
	night-default-label-color: purple
```

If no such attribute is found, the `default-label-color` attribute name is looked up, without the `theme-` prepended.

### Theme Usage

When a theme is defined, its name is prepended to *some* default values.

Default values affected by themes are:

- `cockpit-color` and `cockpit-texture`
- All attributes that end with `color`. Example `default-color`.
- All attributes that end with `texture`. Example `annunciator-bg-texture`.
- All attributes that end with or contain the word `font`. Example `label-font`.

```
theme: light
# Alternate theme is 'dark'
default-label-color: white
dark-default-label-color: coral
```

It is advisable to define a light/day theme and a dark/night theme. There is an activation to change the theme. It also is possible to change the theme with an Observable. For example, it is possible to collect the simulation date, time, and location, and determine if it is a day-light or night time and set the theme accordingly.

## Documentation

Documentation files can be placed in the folder `resources/docs` in a textual form (including markdown), or as PDF.
