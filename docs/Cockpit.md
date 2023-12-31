The Cockpit is the container entity of Cockpitdecks. *For a given aircraft*, it locates deck definitions, loads them and prepares each deck for use with X-Plane.

# Aircraft Configuration

Decks are particular to an aircraft. Cockpitdeck will look for a folder named `deckconfig` in the X-Plane folder of that aircraft.

Cockpidecks will first look for a file named `config.yaml` in the `deckconfig` folder. In that file, Cockpitdecks will find all decks connected to the system and available to it. It will also find a few global parameters.

Cockpidecks will also look for a file named `secret.yaml` in the `deckconfig` folder. In that file, Cockpitdeck will find information such as the serial numbers of the deck devices used. Serial numbers are needed to distinguish all decks of a similar type.

Configuration files for decks are all Yaml-formatted files.

```
XPlaneAircraftFolder
  (...)
  ⊢ deckconfig
    ⊢ config.yaml
    ⊢ secret.yaml
    ⊢ resources
      ⊢ icons
        ⊢ icon-off.png
        ⊢ icon-on.jpg
	  ⊢ fonts
	    ⊢ B612-Regular.otf
	    ⊢ DIN.ttf
      ⊢ docs
        ⊢ README.md
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
### Resources
Resources are fonts, icons, other images, wallpapers, documentation, and texts used and related to that aircraft.
#### Icons
Folder containing all icon images for that aircraft. Images should be in JPEG or PNG format. Typical icon size is 256 × 256 pixels, RGB(A). Icons are named after their file name without the extension.
#### Fonts
Folder containing specific fonts used for that aircraft.
Typical fonts include standard, formal font like DIN, icon fonts like FontAwesome or WeatherIcon font, or fancier font like 7-bar LED fonts. Fonts are named by their file name without extension for TrueType fonts. Open Type Fonts need to supply their extension `.otf`.
#### Docs
Docs folder may contain documentation files, like explanatory images, and descriptive texts. Simpler text or markdown files are preferred.
### Layout Folders
Newt to the the above *resources* folder, there will be one folder per Layout for a deck.
[[Layout|Layout folders]] are explained separately.

# Config.yaml File

The file named `config.yaml` in the `deckconfig` folder contains declarations for each [[Deck|deck]] that will be used and global, aircraft-level attributes.

```
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
The parameters apply to all decks for the given aircraft.

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
If a texture is not found at a lower level, the texture of the level above is used.
If no texture is found, a uniform `cockpit-color` icon is used.
### default-wallpaper-logo
Name of image file, located in the `resource` folder, to be loaded when the deck is not used.

### default-*
Name of default values of several parameters, defined at the aircraft-level. These values will be used for all missing values. They can be raffined at Layout and Page level if necessary.
If no aircraft-level global parameter values are not provided, Cockpitdecks will use its own internal default values.

### decks
A list of deck structure, one per [[Deck|deck]].

Yaml allow for other attributes in the file. They are ignored by Cockpitdecks. You may include other attributes like aircraft name, ICAO code, descriptions, notes, even change log of your file.

# Secret.yaml File
The secret.yaml file contains the serial numbers of your connected decks.
If you have more than one deck of the same type (i.e. two Streamdecks, two X-TouchMini, etc.) this file is mandatory to distinguish between the two physical devices. Otherwise, it is optional.

```
# My decks and their serial numbers.
# Format:
# DeckName: Serial number
# DeckName must match name given in config.yaml.
XPLive: AAA0000000000000000000A0000
```

# Global Cockpitdecks Configuration

Configuration et customization is organized in a **hierarchical** way. The the top, highest level is Cockpitdecks, the application.

Under Cockpitdecks is the Cockpit. If not redefined after, all attributes defined in the Cockpit are used for all other elements.

Under the Cockpit is the Deck. This allow to change a few parameters for each deck. For example, a given deck might have large icons, requiring the use of a large default font size.

Under the Deck is the Layout. This allow to change a few parameters for each layout, like the background color of a panel. Layout parameters are melted into Deck parameters.

Under the Layout is the Page, to control page-level specificities, like, for exemple, a specific background color or texture for a special page.

That's it. At the "lowest" level, at the Button level, there are no default global values, just the values the button will use.
![[hierarchy.png]]
This hierarchical organisation is exploited in [[Button Attribute Default Values|default value lookups]].

## Resources Folder

`deckconfig` folders reside in X-Plane aircraft folders and are specific to that aircraft.
In addition to these aircraft specific folders, Cockpitdecks has a global configuration folder called `resources` located where the Cockpitdecks software resides.

`resources` folder located where the Cockpitdecks software resides contains the following files and subfolders:

## config.yaml
This is a global level configuration file. It always is loaded first and can be overwritten by aircraft, deck, or page-specific variants.

## icons
Icons in this folder are available to all aircrafts.

## fonts
Fonts in this folder are available to all aircrafts.
Cockpitdecks provides a few fonts found here and there together with their respective copyright files.

## docs
A copy of Cockpitdecks documentation is included there. The documentation folder produced in the GitHub wiki of Cockpitdecks.

## Image files
The resource folder contains a few image files used as logos and wallpapers.
There is also an image with color names that can be used in `color` attributes.

## iconfonts.py
This file defines icon fonts. Icon fonts are fonts that are used to display iconic characters often named intuitively. Cockpitdecks comes with a copy of Font Awesome icons, and Weather Icons.

## constants.py
Defines a few constants that should never be changed. Change at your own risk.


> [!ABSTRACT]
> In summary, a Cockpit is defined for a given X-Plane aircraft.
> A Cockpit contains one or more Decks. Each Deck represent a physical deck device used to interact with X-Plane. A Deck is assigned a Layout. A Layout is a collection of Pages. Each Page defines what knobs and buttons on the physical deck do, when they are turned or pushed, and what information they displays in return (through images or LEDs), information extracted from X-Plane data and behavior.
> 