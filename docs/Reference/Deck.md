A Deck represents a deck device connected to the computer, be it a

- Elgato Stream Deck (several models supported),
- Loupedeck Loupedeck Live, or
- Berhinger X-Touch Mini,
- A web deck represented in a web page,

a device that will be used to interact with the X-Plane flight simulator.

A Deck uses and displays a collection of buttons called a Page of buttons. A Deck can display different pages of buttons at different times; a button on a page can be assigned to load another page of buttons.

Each [[Page]]  define the set of buttons on the deck device, what each button does when pressed or turned, and how it will appear on the deck device. The collection of pages that can be installed on a deck is called the [[Layout]] of the deck.

In addition to the Layout and the Pages it contains, a Deck defines deck-level attributes, such as the overall brightness of the device, or how to fill unused or undefined buttons.

# Deck Definition

Decks are declared in the `config.yaml` file in the `deckconfig` folder in the `decks` attribute. The `decks` attributes contains one or more decks as defined by the following attributes:

## Deck Attributes

| Attribute               | Definition                                                                                                                                                                                                                                                       |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`                  | **Mandatory**. Name of the deck. Must match the entry in secret.yaml file, it any.<br>                                                                                                                                                                           |
| `type`                  | **Mandatory**. Type of deck. This points at a very precise deck brand and model. The value must match one of the deck types Cockpitdecks recognizes. The types of deck models Cockpitdecks recognizes is displayed upon startup of the Cockpitdecks application. |
| `layout`                | Optional. Name of the layout for this deck. Default to name value `default`. See the next Section for more information.                                                                                                                                          |
| `brightness`            | Optional. Overall brightness of deck. Default to 100%. It might be necessary to adjust brightness at night or in low light environment.                                                                                                                          |
| `disabled`              | Boolean value to tell whether the deck should be enabled or not for this aircraft. Useful during development process.                                                                                                                                            |
| `default-homepage-name` | Optional. Name of the page to load first in the layout. Default to `index`. That's why layout folder often contains a `index.yaml` page.                                                                                                                         |
| `default-*`             | Optional. Default attributes to use for deck.                                                                                                                                                                                                                    |

Announce of available deck types on startup.

```
cockpit.py: loaded 19 deck types (Virtual Deck for Development, X-Touch Mini, Virtual X-Touch Mini, LoupedeckLive, virtual loupedeck.ct, virtual loupedeck.live.s, Virtual LoupedeckLive with Mosaic, Virtual LoupedeckLive, Stream Deck Original, Stream Deck Mini, Stream Deck Neo, Stream Deck +, Stream Deck XL, Streamdeck, Virtual Streamdeck Mini, Virtual Streamdeck MK.2, Virtual Stream Deck Neo, Virtual Streamdeck +, Virtual Streamdeck XL), 11 are virtual deck types
```

# Deck Layout

For a given aircraft, a deck has a Layout. The [[Layout]] of a deck is the collection of [[Page|Pages]] that will be used and displayed on the deck device for that aircraft. All these pages are grouped into a folder called a layout. A Layout is a folder in the `deckconfig` folder that contains *pages*.

``` hl_lines="8"
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

If there is no physical deck device, Cockpitdecks can be used to replicate one of those on a web page.

To use an emulation of a Stream Deck device for example, it is necessary to install the streamdeck component of Cockpitdecks. When done, it is possible to create virtual Stream Deck devices and display them in a web page.

This is equally possible with other brands like Loupedeck or Behringer.
