A Layout is a collection of *Pages* that will be displayed on that deck.

Layouts were created to cope with different deck models. If you have a set of 30 commands, you can display them all on a 32 key deck on the same *page*. But you have to spread 30 commands on *two pages* of buttons if your deck can only display 16 buttons at a time. Same commands, but two different *layouts*.

A Layout is a folder, inside the `deckconfig` main folder. The Layout is named and addressed by the name of that folder.

# Layout Folder

The Layout folder contains the following files:

```
  ⊢ live
    ⊢ config.yaml
    ⊢ page1.yaml
    ⊢ page2.yaml
```

The Layout name is `live`, it contains 2 pages.

# Layout `config.yaml` File

The `config.yaml` file inside a layout folder defines Layout-level attributes. The file is optional.

```yaml
# This is at layout level
default-icon-color: (94, 111, 130)
default-label-color: blue
default-label-font: DIN Bold.ttf
default-label-size: 13
default-page-name: page1
```

## Attributes

| Attribute                       | Definition                                          |
| ------------------------------- | --------------------------------------------------- |
| `default-icon-color: blue`      | Optional. Default color to use for icon background. |
| `default-label-color: white`    | Optional. Default color to use for layout labels.   |
| `default-label-font: D-DIN.otf` | Optional. Default font to use for layout labels.    |
| `default-label-size: 13`        | Optional. Default label size.                       |
| `default-homepage-name`         | Optional. Default page name in layout.              |

The default values of attributes (like font, colors, and sizes) are fetched at the Cockpit level if they are not specified at the Layout level.

Layout attributes are used for all pages in the layout, unless a Page refines the definition of one of these attribute.

# Pages

All other Yaml files in the folder are considered to be [[Page|Pages]] in the layout.

# Default Layout

If a deck has no layout specified, Cockpitdecks will generate one with one default page that will display a logo image on the deck (if it is capable of displaying images...) and use the first available push button to toggle X-Plane Map On or Off.
