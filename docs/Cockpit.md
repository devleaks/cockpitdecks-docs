The Cockpit is the *Maestro* of the Cockpitdecks application.

It starts up the entire Cockpitdecks application. It establishes connection to the simulator, scans for existing decks connected to the computer, and load the aircraft configuration files. It listen to interactions that occur on the decks and listen to simulator changes to reflect deck statuses. It also monitors with aircraft is currently loaded in the simulator, and if the user changes aircraft, it loads the new configuration if any.

# Aircraft Configuration

Decks are particular to one aircraft. Cockpitdeck will look for a folder named `deckconfig` in the X-Plane folder of that aircraft. If none is found, it will load a default page, with defaults actions on each deck available to it.

If a `deckconfig` folder is found in the folder of the aircraft currently used, Cockpidecks will first look for a file named `config.yaml`. In that file, Cockpitdecks will find all decks connected to the system and available to it. It will also find a set of global parameters.

Cockpidecks will also look for a file named `secret.yaml` in the `deckconfig` folder. In that file, Cockpitdeck will find information such as the serial numbers of the deck devices used. Serial numbers are needed to distinguish all decks of a similar type.

All configuration files for decks are all Yaml-formatted files. Yaml file contains a structured list of name, value pairs. The name is referred to as an *attribute*. The *value* can be almost anything: A number, a string, a list of things, or a list of other attributes.

# Deckconfig Folder

Here is the overall structure of the files and sub-folders inside the `deckconfig` folder.

``` hi  hl_lines="3"
<X-Plane Aircraft Folder>
  (.. aircraft files ..)
  ⊢ deckconfig
    ⊢ config.yaml
    ⊢ secret.yaml
    ⊢ resources
      ⊢ icons
        ⊢ icon-off.png
      ⊢ fonts
        ⊢ B612-Regular.otf
        ⊢ DIN.ttf
      ⊢ docs
        ⊢ README.md
      ⊢ other-resource.py
    ⊢ layout1
      ⊢ config.yaml
      ⊢ page1.yaml
      ⊢ page2.yaml
      ...
    ...
    ⊢ layoutN
      ⊢ page1.yaml
      ⊢ page2.yaml
      ...
```

The  `deckconfig` folder contains the following files and sub-folders:

| Name         | Description                                                                                                                       | Note |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------- | ---- |
| config.yaml  | Main configuration file                                                                                                           |      |
| secreat.yaml | Serial numbers of decks used by Cockpitdecks                                                                                      |      |
| resources    | Resource files used by this configuration. Resources are icons, fonts, images, etc.                                               |      |
| layout       | A Layout is a folder that contains what is displayed on a deck. There can be as many Layout Folder as necessary for this aircraft |      |
| docs         | Documentation relative to this aircraft and configuration                                                                         |      |
|              |                                                                                                                                   |      |

# config.yaml File

The file named `config.yaml` in the `deckconfig` folder contains declarations for each [[Deck|deck]] that will be used and global, aircraft-level attributes.

```yaml
# Definition of decks for Toliss A321
#
aicraft: Toliss A321
decks:
  - name: XPLive
    type: loupedeck
    layout: live
    brightness: 70
# These attributes are default values at global level
default-wallpaper-logo: Airbus-logo.png
default-icon-color: (94, 111, 130)
default-label-color: white
default-label-font: DIN.ttf
default-label-size: 14
cockpit-color: lightblue
```

## Attributes

The parameters apply to all decks for the given aircraft. Each individual deck will have the possibility to redefine these values if necessary.

### aircraft

Optional information. The name of the aircraft for this set of deck.

### cockpit-color

Color for the cockpit. This color is used as the default background color for icons.

### cockpit-texture

Name of a image file (JPEG or PNG) that will be used as the (default) background of icons.

The `cockpit-texture` file will be searched at different places depending on where it is specified.

The `cockpit-texture` file can be specified at the Cockpit, Deck, Page, or Button level.

Cockpit-level default textures will be seached in the following folders (in that order):

- `resources`
- `resources/icons`

If the AIRCRAFT is specified, Cockpit-level textures and all other levels textures will be searched in the following folders:

- `AIRCRAFT/resource`
- `AIRCRAFT/icons`

If no texture is found, a uniform `cockpit-color` icon is used.

### default-wallpaper-logo

Name of image file, located in the `resource` folder, to be loaded when the deck is not used.

### default-*

Name of default values of several parameters, defined at the aircraft-level. These values will be used for all missing values. They can be raffined at Layout and Page level if necessary.

If no aircraft-level global parameter values are not provided, Cockpitdecks will use its own internal default values.

### decks

A list of deck structure, one per [[Deck|deck]].

Yaml allow for other attributes in the file. They are ignored by Cockpitdecks. You may include other attributes like aircraft name, ICAO code, descriptions, notes, even change log of your file.

# secret.yaml File

The `secret.yaml` file contains the serial numbers of your connected decks.

If you have more than one deck of the same type (i.e. two Streamdecks, two X-TouchMini, etc.) this file is mandatory to distinguish between the two physical devices. Otherwise, it is optional.

```yaml
# My decks and their serial numbers.
# Format:
# DeckName: Serial number
# DeckName must match name given in config.yaml.
XPLive: AAA0000000000000000000A0000
```

# Resources

Resources are fonts, icons, other images, wallpapers, documentation, and texts used and related to that aircraft.

## Icons

Folder containing all icon images for that aircraft. Images should be in JPEG or PNG format. Typical icon size is 256 × 256 pixels, RGB(A). Icons are named after their file name without the extension.

## Fonts

Folder containing specific fonts used for that aircraft.

Typical fonts include standard, formal font like DIN, icon fonts like FontAwesome or WeatherIcon font, or fancier font like 7-bar LED fonts. Fonts are named by their file name without extension for TrueType fonts. Open Type Fonts need to supply their extension `.otf`.

## Docs

Docs folder may contain documentation files, like explanatory images, and descriptive texts. Simpler text or markdown files are preferred.

# Layout Folders

Next to the the above *resources* folder, there will be one folder per Layout for a deck.

[[Layout|Layout folders]] are explained separately.
