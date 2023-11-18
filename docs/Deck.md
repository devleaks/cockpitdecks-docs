A Deck represents a physical device connected to the computer, be it a

- Elgato Stream Deck, 
- Loupedeck Loupedeck Live, or
- Berhinger X-Touch Mini,

a device that will be used to interact with the X-Plane flight simulator.

A Deck uses and displays a collection of buttons called a Page of buttons. A Deck can display different pages of buttons at different times; a button on a page can be assigned to load another page of buttons.

Each [[Page]]  define the set of buttons on the deck device, what each button does when pressed or turned, and how it will appear on the deck device. The collection of pages that can be installed on a deck is called the [[Layout]] of the deck.

In addition to the Pages it contains, a Deck defines deck-level attributes, such as the overall brightness of the device, or how to fill unused or undefined buttons.

# Definition
Decks are declared in the `config.yaml` file in the `deckconfig` folder in the `decks` attribute. The `decks` attributes contains one or more decks as defined by the following attributes:

## Deck Attributes

`name: StreamdeckMK2`
Mandatory. Name of the deck.

`type: streamdeck`
Optional. Type of deck. Streamdeck, Loupedeck, or Xtouchmini.

`layout: layout_folder_name`
Optional. Name of the layout for this deck. Default to `default`.

`brightness: 80`
Optional. Overall brightness of deck. Default to 100%.

`default-label-font: DIN Medium.ttf`
Optional. Default font to use for deck. Default to DIN, which is provided with Cockpitdecks.

`default-label-size: 13`
Optional. Default label size. Cockpitdecks manipulates icon 256x256 pixels.

`default-homepage-name`
Optional. Default page name in layout. Default to `index`.

> Note: The type of Deck defines the "device driver" that will be used to talk to the deck. If a new model of deck appears in the future, it will be necessary to design its "device driver" for Cockpitdecks to use it.

# Deck Layout
For a given aircraft, a deck has a Layout. The [[Layout]] of a deck is a collection of [[Page|Pages]] that will be used and displayed on the deck device.
A Layout is a folder in the `deckconfig` folder.

```
XPlaneAircraftFolder
  (...)
  ⊢ deckconfig
    ⊢ icons
    ⊢ fonts
    ⊢ layout1
      ⊢ config.yaml
      ⊢ page1.yaml
      ⊢ page2.yaml
      ...
    ...
```

The deck definition should contain a `layout` attribute that indicates which layout will be used for that deck. The default layout name, if not indicated is `default`. If no layout is found for the deck, a default, minimal layout is created.

