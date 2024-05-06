For a given deck, a Layout is a collection of *Pages* that will be displayed on that deck.
A Layout is a folder, inside the `deckconfig` main folder.
The Layout is named and addressed by the name of the folder.

Layouts were created to cope with different deck models. If you have a set of 30 commands, you can display them all on a 32 key deck. But you have to make two pages of buttons on a 16 key deck. Same button definitions, but two *layouts*.

# Layout Folder
The Layout folder contains the following files:

```
    ⊢ layout_name
      ⊢ config.yaml
      ⊢ page1.yaml
      ⊢ page2.yaml
```

# Config.yaml

The `config.yaml` file inside a layout folder defines Layout-level attributes. The file is optional.

```yaml
# This is at layout level
default-icon-color: (94, 111, 130)
default-label-color: blue
default-label-font: DIN Bold.ttf
default-label-size: 13
default-page-name: index
```

## Attributes

The default value for each attribute is the value found the Cockpit attributes.

`default-icon-color: blue`
Optional. Default color to use for icon background.

`default-label-color: white`
Optional. Default color to use for layout labels.

`default-label-font: DIN Medium.ttf`
Optional. Default font to use for layout labels.

`default-label-size: 13`
Optional. Default label size.

`default-homepage-name`
Optional. Default page name in layout.

The default value of some attributes (like font, colors, and sizes) is fetched at the Cockpit level if they are not specified at the Layout level.

# Pages

Other files in the folder are considered to be [[Page|Pages]] in the layout.

# Default Layout

If a deck has no layout specified, Cockpitdecks will generate one with one default page that will display a logo image on the deck (if it is capable of displaying images...) and use the first available push button to toggle X-Plane Map On or Off.