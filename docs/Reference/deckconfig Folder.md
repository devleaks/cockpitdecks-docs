# The `deckconfig` Folder

The overall structure of the files and sub-folders inside the `deckconfig` folder is as follow:

```tree
<X-Plane Aircraft Folder>/
  (.. aircraft files ..)
  deckconfig/
    config.yaml
    secret.yaml
    resources/
      icons/
        icon-off.png
      fonts/
        B612-Regular.otf
        DIN.ttf
      decks/
        images
        types
      docs/
        README.md
      other-resource.py
    layout1/
      config.yaml
      page1.yaml
      page2.yaml
      ...
    ...
    layoutN
      page1.yaml
      page2.yaml
      ...
```

The  `deckconfig` folder contains the following files and sub-folders:

| Name          | Description                                                                                                                                                                                                                                         |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `config.yaml` | Main configuration file                                                                                                                                                                                                                             |
| `secret.yaml` | Serial numbers of decks used by Cockpitdecks                                                                                                                                                                                                        |
| resources     | Resource files used by this configuration. Resources are icons, fonts, images, etc.                                                                                                                                                                 |
| layout(s)     | A Layout is a folder that contains what is displayed on a deck. There can be as many Layout Folder as necessary for this aircraft. All remaining folders in the `deckconfig`folder are layout folders. There usually is one Layout folder per deck. |

# `config.yaml` File

The file named `config.yaml` in the `deckconfig` folder it the main configuration file, It contains declarations for each [[Deck]] that will be used, and global, aircraft-level attributes.

```yaml
# Definition of decks for Toliss A321
#
aircraft: Toliss 
decks:
  - name: XPLive
    type: loupedeck
    layout: live
    brightness: 70
# These attributes are default values at global level
named-colors:
    COCKPIT_BACKLIGHT: darkorange
default-wallpaper-logo: Airbus-logo.png
default-icon-color: (94, 111, 130)
default-label-color: white
default-label-font: DIN.ttf
default-label-size: 14
cockpit-color: lightblue
```

## Attributes

The following attributes apply to all decks for the given aircraft. Each individual deck will have the possibility to redefine these values if necessary.

| Attribute                | Definition                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `aircraft`               | Optional information. The name of the aircraft for this set of deck.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| `named-colors`           | Allows to introduce your own named color. You can then use this name as a color. The value of the named color can either be a 3 or 4 value tuple (r, g, b, a), or the name of a Pillow color. (See [[Resources]].)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| `cockpit-color`          | Color for the cockpit. This color is used as the default background color for icons.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| `cockpit-texture`        | Name of a image file (JPEG or PNG) that will be used as the (default) background of icons.<br><br>The `cockpit-texture` file will be searched at different places depending on where it is specified.<br><br>The `cockpit-texture` file can be specified at the Cockpit, Deck, Page, or Button level.<br><br>Cockpit-level default textures will be seached in the following folders (in that order):<br><br>- `resources`<br>- `resources/icons`<br><br>If the AIRCRAFT is specified, Cockpit-level textures and all other levels textures will be searched in the following folders:<br><br>- `AIRCRAFT/resource`<br>- `AIRCRAFT/icons`<br><br>If no texture is found, a uniform `cockpit-color` icon is used. |
| `default-wallpaper-logo` | Name of image file, located in the `resource` folder, to be loaded when the deck is not used.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| `default-*`              | Name of default values of several parameters, defined at the aircraft-level. These values will be used for all missing values. They can be raffined at Layout and Page level if necessary.<br><br>If no aircraft-level global parameter values are not provided, Cockpitdecks will use its own internal default values.                                                                                                                                                                                                                                                                                                                                                                                          |
| **decks**                | A list of [[Deck]] structure, one per deck.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

Yaml allow for other attributes in the file. They are ignored by Cockpitdecks. You may include other attributes like aircraft name, ICAO code, descriptions, notes, even change log of your file. Comments are also allowed in Yaml files.

### See Also

[[Main config.yaml File]]

# `secret.yaml` File

The `secret.yaml` file contains the serial numbers of your connected decks.

If you have more than one deck of the same type (i.e. two Streamdecks, two X-TouchMini, etc.) this file is mandatory to distinguish between the two physical devices. Otherwise, it is optional.

```yaml
# My decks and their serial numbers.
# Format:
# DeckName: Serial number
# DeckName must match name given in config.yaml.
XPLive: AAA0000000000000000000A0000
```

> [!WARNING] Serial Numbers
> We experimentally noticed that serial numbers as displayed on the device and as reported by some software package do not always correspond. This also depends on the operating system. As a practical illustration, for HID devices, the request for its serial number throught HID specific protocol calls returns a value WA1234MHK0G, while the core operating system raw USB device probe returns A00WA1234MHK0G. Some device do not respond to serial number requests and in this case, an arbitrary software dependent value is returned, and not guaranteed to be unique(!).
> As a consequence, Cockpitdecks maintains a list of valid serial numbers for a given device.

# Resources

Resources are fonts, icons, other images, wallpapers, documentation, and texts used and related to that aircraft. All these elements are organized into the `resources` folder.

Usually, the resources folder contains the following sub-folders:

| Folder  | Content                                                                                                                                                                                                  |
| ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `icons` | Folder containing all icon images for that aircraft. Images should be in JPEG or PNG format. Typical icon size is 256 × 256 pixels, RGB(A). Icons are named after their file name without the extension. |
| `fonts` | Folder containing all fonts for that aircraft. Fonts can be Truetype or Opentype. Fonts are named after their file name with the extension.                                                              |
| `docs`  | Docs folder may contain documentation files, like explanatory images, and descriptive texts. Simpler text or markdown files are preferred.                                                               |
| `decks` | Cockpitdecks allow experienced users to create their own [[Web Decks]] specific to an aircraft.                                                                                                          |

# Layout Folders

Next to the the above *resources* folder, there will be one folder per Layout for a deck.

[[Layout|Layout folders]] are explained later.

# About Configuration Files: Yaml

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
