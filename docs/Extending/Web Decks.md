In addition to real physical decks, Cockpitdecks offers *Web Decks* rendered in a browser window.

![[virtualdecks.jpg]]

This is very handy to design layout for deck a developer does not necessary owns.

Virtual decks need to be declared at two places:

1. [[Deck Internals|Definition of the deck]], number of buttons, icon size, and layout of buttons. This allows Cockpitdecks to determine the virtual deck capabilities and key arrangement for drawing, like any other real physical deck.
2. *Use of the deck*, like any regular deck, with its Layout and is configuring parameters. To use a web deck it must appear in the list of decks in the `deckconfig/config.yaml` file of an aircraft.

Web Decks communicate with Cockpitdecks through Websockets. A WebSocket proxy server control Interactions between Web Decks and Cockpitdecks.

# Web Deck Definition

Virtual decks are declared like any other deck. Their driver must be `virtualdeck`.

```yaml hl_lines="2 9 "
type: Virtual Deck 3×2
driver: virtualdeck
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
  overlays:
    - type: text
      text: Hello, world
      position: [840, 40]
      font: anexistingwebfontalreadyloaded
      size: 20
      color: white
    - type: image
      image: logo-transparent.png
      position: [840, 40]
```

For web decks, the mandatory `background` sets the appearance of the deck in a web page.

Web decks can have the following types of interactive buttons:

1. Keys (simple press, long press, etc.)
2. Encoders (turned clockwise, counter clockwise)
3. Touchscreen (pressed, swiped)
4. Slider (slid between 2 range values)

As feedback, Web Decks display images as provided by Cockpitdecks. Even is a deck button has a complex representation, an image of it will be built be Cockpitdecks and sent for display on the web deck.

# Web Deck usage

To use a web deck for a given aircraft, it need to be declared in the list of decks available for that aircraft.

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
    layout: test versioN
```

If several virtual decks are running, it is necessary, like other decks, to specify its serial number in the secret.yaml file.

> [!WARNING] Serial Number of Virtual Decks
> The serial number of virtual decks must be specified in the secret.yaml file.

```yaml 
Vdeck2: 7702
Vdeck1: 7701
```

Web decks include their name in the messages they send to Cockpitdecks. This allows Cockpitdecks to identify the deck that sent the message.

Web Decks can run on another computer than the one running X-Plane and/or Cockpitdecks.

To start web decks, it is advised to first start Cockpitdecks so that web decks can be discovered, then simply issue:

```sh
$ python bin/webdeck_start.py
```

Web Decks are automatically and dynamically discovered when Cockpitdecks runs.

Recall, to start Cockpitdecks:

```sh
$ python bin/cockpitdecks_upd_start.py aircrafts/A321
```

Web Decks are started in their own processes that does not interfere with Cockpitdecks.

# Virtual Web Decks Internals

*Web Decks* are designed with simple standard web features, are rendered on an HTML Canvas, uses standard events to report interaction through basic JavaScript functions.
A Proxy application is necessary between Cockpitdecks and the browser to convert TCP/IP socket requests into WebSocket requests that can be understood by browsers.
The application that serves them is a very simple Flask application (2 routes) with 2 simple Ninja2 templates. The Flask application also runs the WebSocket proxy.

![[webdecks.svg|600]]

See [[Deck Internals#Folder Organisation]].

### Web Deck List (Web Deck Home Page)

![[webdeck-list.png]]

### Web Deck Example (Including Background Image)

![[webdeck-example.png]]
