In addition to real physical decks, Cockpitdecks offers *Web Decks* rendered in a browser window.

![[virtualdecks.jpg]]

This is very handy to design layout for deck a developer does not necessary owns.

Virtual decks need to be declared at two places:

1. [[Deck Internals|Definition of the deck]], number of buttons, icon sizes, and layout of buttons. This allows Cockpitdecks to determine the virtual deck capabilities and key arrangement for drawing, like any other real physical deck.
2. *Use of the deck*, like any regular deck, with its Layout and is configuring parameters. To use a web deck it must appear in the list of decks in the `deckconfig/config.yaml` file of an aircraft.

Web Decks communicate with Cockpitdecks through Websockets. A WebSocket proxy server control Interactions between Web Decks and Cockpitdecks.

# Web Deck Definition

Virtual decks are defined like any other deck. Their driver must be `virtualdeck`. Also, the contains a few additional attributes to display them on a HTML web Canvas.

```yaml
name: Virtual Deck 3×2
driver: virtualdeck
buttons:
    - index: 0
      type: key
      position: [55, 25]
      display: [96, 96]
      color: white
      options: rounded=8

	  ...

background:
  color: "#222"
  image: loupedeck.live.png
  size: [800, 600]
  overlays:
    - text: Hello, world
      position: [840, 40]
      font: anexistingwebfontalreadyloaded
      size: 20
      color: white
    - image: logo-transparent.png
      position: [840, 40]
```

Please refer to the [[Deck Internals]] for references on how to create and declare web decks.

# Web Deck Usage

## Web Deck Declaration

To use a web deck for a given aircraft, it need to be declared in the list of decks available for that aircraft.

###  `deckconfig/config.yaml`

```yaml
# Definition of decks for Toliss A321
#
aicraft: Toliss 
decks:
  - name: Vdeck1
    type: Virtual Deck 3×2
    layout: devlayout
```

The same virtual deck definition can be used more than once to create similar decks:

```yaml 
decks:
  - name: Vdeck2
    type: Virtual Deck 3×2
    layout: test version
```

Web decks include their name in the messages they send to Cockpitdecks. This allows Cockpitdecks to identify the deck that sent the message.

### `deckconfig/secret.yaml`

If several virtual web decks are running, it is necessary, like other decks, to specify their serial numbers in the secret.yaml file.

> [!NOTE] Serial Number of Virtual Web Decks
> The serial number of virtual decks must be specified in the secret.yaml file. It can be any number or string, but it must be different for each virtual web deck.

```yaml 
Vdeck1: deck1
Vdeck2: deck2
```

## Web Deck Server

Web Decks can run on another computer than the one running X-Plane and/or Cockpitdecks.

To start the web decks server application, it is advised to first start Cockpitdecks so that web decks can be discovered, then simply issue:

```sh
$ python bin/webdeck_start.py
```

Virtual Web Decks are automatically and dynamically discovered when Cockpitdecks runs.

Recall, to start Cockpitdecks:

```sh
$ python bin/cockpitdecks_upd_start.py aircrafts/A321
```

Web Decks are started in their own processes that does not interfere with Cockpitdecks.

If interested, please refer to the [[Deck Internals#Virtual Web Decks Internals|Virtual Web Deck Internals]].

### Web Deck List (Web Deck Home Page)

When started, the virtual web decks server offers a Welcome page with all virtual web decks available to the user. Selecting a deck starts it in another web navigator window.

![[webdeck-list.png]]

### Web Deck Example

Here is a virtual web deck, carefully designed to represent an existing Elgato Stream Deck MK.2 deck. Cheaper.

![[webdeck-example.png]]
