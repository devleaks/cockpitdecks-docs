In addition to real physical decks, Cockpitdecks offers *Web Decks* rendered in a browser window.

![[virtualdecks.jpg]]

This is very handy to design layout for deck a developer does not necessary owns.

Virtual decks need to be declared at two places:

1. [[Deck Internals|Definition of the deck]], number of buttons, icon sizes, and layout of buttons. This allows Cockpitdecks to determine the virtual deck capabilities and key arrangement for drawing, like any other real physical deck.
2. *Use of the deck*, like any regular deck, with its Layout and is configuring parameters. To use a web deck it must appear in the list of decks in the `deckconfig/config.yaml` file of an aircraft.

Web Decks communicate with Cockpitdecks through Websockets. A WebSocket proxy server control Interactions between Web Decks and Cockpitdecks.

# Web Deck Definition

Virtual decks are declared like any other deck. Their driver must be `virtualdeck`.

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

## Deck Type Definition Attributes

### Name

Name of virtual web deck type.

### Driver

Driver of virtual deck type. Must be `virtualdeck`.

### Buttons

List of Deck Type Button Definition Block.

See below.

### Background

For web decks, the mandatory `background` sets the appearance of the deck in a web page.

As feedback, Web Decks display images as provided by Cockpitdecks. Even is a deck button has a complex representation, an image of it will be built be Cockpitdecks and sent for display on the web deck.

## Deck Type Button Definition Block

A Deck Type Button Definition Block is

- either the definition of a single button,
- or the definition of a list of buttons of similar types organized in an array.

Web decks can have the following types of interactive buttons:

1. Keys (simple press, long press, etc.)
2. Encoders (turned clockwise, counter clockwise)
3. Touchscreen (pressed, swiped)
4. Slider (slid between 2 range values)

### Single Button Definition

```yaml
name: Virtual Deck
driver: virtualdeck
buttons:
  - name: left
    action: [push, swipe]
    feedback: image
    dimension: [52, 270]
    layout:
      offset: [96, 78]
    options: corner_radius=4
```

#### Name

Name of button

#### Action

List of possible action of button

#### Feedback

List of possible feedback of button

#### Dimension

Dimension of button.

If dimension is an array of 2 values, it is the width and height of a rectangular button.

If the dimension is a single value, it is the radius of a circular shaped button.

#### Options

Comma-separated list of options, a single option can either be a name=value, or just a name, in which case the value is assumed to be True.

`options: count=8,active`

sets options `count`to value 8, and `active` to True.

#### Layout

Layout of the buttons on the web deck canvas.

##### Offset

##### Spacing

Buttons will be arranged at regular interval, starting from Offset, with supplied spacing between the buttons.

### Multiple Button Definition

```yaml
name: Virtual Deck
driver: virtualdeck
buttons:
  - name: 0
    prefix: e
    repeat: [1, 3]
    action: [encoder, push]
    dimension: 27
    layout:
      offset: [45, 115]
      spacing: [0, 41]
  - name: 3
    prefix: e
    repeat: [1, 3]
    action: [encoder, push]
    dimension: 27
    layout:
      offset: [624, 115]
      spacing: [0, 41]
```

#### Repeat

Number of time the same button is replicated along width (x) and height (y) axis.

If only one value is supplied, it is supposed to be `[value, 1]` array. For vertical layout, specify `[1, value]` instead.

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
    layout: test version
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
