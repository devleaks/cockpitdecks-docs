A Deck represents a physical device connected to the computer, be it a

- Elgato Stream Deck (several models supported),
- Loupedeck Loupedeck Live, or
- Berhinger X-Touch Mini,

a device that will be used to interact with the X-Plane flight simulator.

A Deck uses and displays a collection of buttons called a Page of buttons. A Deck can display different pages of buttons at different times; a button on a page can be assigned to load another page of buttons.

Each [[Page]]  define the set of buttons on the deck device, what each button does when pressed or turned, and how it will appear on the deck device. The collection of pages that can be installed on a deck is called the [[Layout]] of the deck.

In addition to the Layout and the Pages it contains, a Deck defines deck-level attributes, such as the overall brightness of the device, or how to fill unused or undefined buttons.

# Deck Definition

Decks are declared in the `config.yaml` file in the `deckconfig` folder in the `decks` attribute. The `decks` attributes contains one or more decks as defined by the following attributes:

## Deck Attributes

`name: StreamdeckMK2`

**Mandatory**. Name of the deck. Must match the entry in secret.yaml file, it any.

`type: Stream Deck XL`

**Mandatory**. Type of deck. This points at a very precise deck brand and model. The value must match one of the deck types Cockpitdecks recognizes.

The types of deck models Cockpitdecks recognizes is displayed upon startup of the Cockpitdecks application.

`brightness: 80`

Optional. Overall brightness of deck. Default to 100%. It might be necessary to adjust brightness at night or in low light environment.

`layout: layout_folder_name`

Optional. Name of the layout for this deck. Default to name value `default`. See the next Section for more information.

`default-homepage-name`

Optional. Default page name in layout. Default to `index`.

`default-label-font: DIN Medium.ttf`

Optional. Default font to use for deck. Default to DIN, which is provided with Cockpitdecks.

`default-label-size: 13`

Optional. Default label size. Cockpitdecks manipulates icon 256x256 pixels.

Please note that other default values can be set at the deck level. Please refer to the button common attributes for a list of values that can have default at the deck level.

# Deck Layout

For a given aircraft, a deck has a Layout. The [[Layout]] of a deck is a collection of [[Page|Pages]] that will be used and displayed on the deck device for that aircraft.

A Layout is a folder in the `deckconfig` folder.

```yaml hl_lines="7"
XPlaneAircraftFolder
  (...)
  ⊢ deckconfig
    ⊢ resources
      ⊢ fonts
      ⊢ icons
      ⊢ ...
    ⊢ layout1
      ⊢ config.yaml
      ⊢ page1.yaml
      ⊢ page2.yaml
      ...
    ...
```

The deck definition should contain a `layout` attribute that indicates which layout will be used for that deck. The default layout name, if not indicated is `default`. If no layout is found for the deck, a default, minimal layout is created.

# Web Decks

No deck? [[Web Decks|We got you covered]].
