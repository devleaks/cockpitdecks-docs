In addition to real physical decks, Cockpitdecks offers *Web Decks* rendered in a browser window.

![[virtualdecks.jpg]]

A Web Deck behaves like their physical counterparts, with limitations inherent to presentation in a browser window. In a nutshell, a web deck has an optional background image (or a uniform background color), and buttons laid over it. Buttons can be of four main types:

- Buttons that have an image or iconic representation and that can be pressed,
- Encoders that can be turned, pushed and pulled,
- Surfaces that behaves more or less like touchscreens, touched by the mouse pointer,
- Hardware representation, that are deck-specific images that can be placed to mimic the appearance of the deck, with changing states.

A web deck is added like any other deck.

Like its physical counterpart, it first must be named and defined, that is Cockpitdecks must know how many buttons are available, of what type, how can users interact with those buttons, etc.  While this information is sufficient for physical decks, web decks need a little more information to express how buttons, encoders, and touchscreens are positioned on the deck. That is also where we supply a background image.

Second, the named web deck need to be added to the list of decks available to an aircraft, in the `deckconfig/config.yaml` file.

If a Web Deck is detected in an aircraft configuration, Cockpitdecks will automagically start a basic web server to present the Web Deck to the user. The index (home) page of the web server will present all web decks available to the user for that aircraft. Selecting on web deck will start it in another browser window.

Web decks are very handy to design layout for decks a developer does not necessary owns.

Web decks can be (almost) any physical deck represented in Cockpitdecks (most popular brands and models are actually provided with Cockpitdecks), or any next deck model of your fantasy. As an example and proof of concept, Cockpitdecks comes with a fully operational overhead panel for Toliss A321 neo aircraft. Yes, you read it right, you can have Toliss A321 overhead panel on a touch screen or tablet.

Web decks that reproduce existing physical decks like Stream Decks and Loupedeck LoupedeckLive are bundled together with their physical counterparts. To use virtual Stream Decks, you must install the streamdeck extension to Cockpitdecks.

# Web Deck Declaration and Use

Virtual decks need to be declared at two places:

1. [[Deck Internals|Definition of the deck]], number of buttons, icon sizes, and layout of buttons. This allows Cockpitdecks to determine the web deck capabilities and key arrangement for drawing, like any other real physical deck.
2. *Use of the deck*, like any regular deck, with its Layout and is configuring parameters. To use a web deck it must appear in the list of decks in the `deckconfig/config.yaml` file of an aircraft.

Web Decks communicate with Cockpitdecks through Websockets. A WebSocket proxy server control Interactions between Web Decks and Cockpitdecks.

# Web Deck Definition

Web decks can be defined at two locations. Cockpitdecks defined a few web decks to help development of physical decks. They are provided with Cockpitdecks and are not meant to be altered by users.

However, a cockpit designer may want to add her or his own web deck. To do so, and to prevent adding files to Cockpitdecks source code, it is possible to define additional web deck types in the `deckconfig` folder of an aircraft.

```
deckconfig
  ⊢ resources
    ⊢ decks
	  ⊢ types
	    ⊢ custom_deck_type.yaml
	 ⊢ images
	    ⊢ custom_background_image.png
```

Virtual web decks are defined like any other deck. Their driver must be `virtualdeck`. They contain  additional attributes to render them on a HTML web Canvas like background image or color.

```yaml hl_lines="2"
name: Virtual Deck 3×2
driver: virtualdeck
buttons:
    - index: 0
      action: push
      position: [55, 25]
      feedback: image
      image: [96, 96]
      options: rounded=8
    - ...

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

Please refer to the [[Deck Internals]] for references on how to create and define web decks.

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

The same web deck definition can be used more than once to create similar decks:

```yaml 
decks:
  - name: Vdeck2
    type: Virtual Deck 3×2
    layout: test version
```

Web decks include their name in the messages they send to Cockpitdecks. This allows Cockpitdecks to identify the deck that sent the message.

### `deckconfig/secret.yaml`

If several web decks are running, it is necessary, like other decks, to specify their serial numbers in the secret.yaml file.

> [!NOTE] Serial Number of Web Decks
> The serial number of web decks must be specified in the secret.yaml file. It can be any number or string, but it must be different for each web deck.

```yaml 
Vdeck1: deck1
Vdeck2: deck2
```

## Web Deck Application Server

The Web Deck application server is automagically started when Cockpitdecks starts if there are web decks to serve.

### Web Deck List (Web Deck Home Page)

When started, the web decks server offers a Welcome page with all web decks available to the user. Selecting a deck starts it in another web navigator window.

![[webdeck-list.png]]

### Web Deck Example

Here is a web deck, carefully designed to represent an existing Elgato Stream Deck MK.2 deck. Cheaper.

![[webdeck-example.png]]

### Web Deck Example (Complex)

This example shows that there is no limit on background and deck type definition if you are patient and meticulous. Here is a fully working exemple of Toliss A321neo Overhead Panel virtualized with Cockpitdecks.

#### Deck Type

```yaml hl_lines="4 10"
name: Virtual A321neo Overhead
driver: virtualdeck
buttons:
  - name: apumaster
    action: push
    feedback: image
    dimension: [40, 40]
    layout:
      offset: [560, 855]
  - name: apustart
    action: push
    feedback: image
    dimension: [40, 40]
    layout:
      offset: [560, 918]
background:
  color: black
  image: a321neo.overhead.png
```

#### deckconfig.yaml

```yaml hl_lines="4 10"
decks:
  -	name: A321 Overhead
	type: Virtual A321neo Overhead
	layout: overhead
```

#### overhead/index.yaml Page

```yaml hl_lines="2 16"
buttons:
  - index: apumaster
    name: APUMASTER
    type: push
    annunciator:
      size: full
      model: B
      parts:
        B1:
          text: "ON"
          color: deepskyblue
          framed: true
          size: 60
          formula: ${AirbusFBW/APUMaster}
    command: toliss_airbus/apucommands/MasterToggle
  - index: apustart
    name: APUSTART
    type: push
    annunciator:
      size: full
      model: B
      parts:
        B0:
          text: AVAIL
          color: lime
          formula: ${AirbusFBW/APUAvail}
        B1:
          text: "ON"
          color: deepskyblue
          framed: true
          size: 60
          formula: ${AirbusFBW/APUStarter}
    command: toliss_airbus/apucommands/StarterToggle
```

![[webdeck-a321.png]]

Pay attention to the center lower annunciator, lightly white-framed. They are produced by Cockpitdecks and reflect the status of Toliss' annunciators.

If buttons have been created for other, physical decks, designing the entire overhead panel is a matter of:

1. using Deck Designer to spot all annunciators, encoders, buttons, etc. and NAME them.
2. copy/pasting button deck definitions and renaming the button index to the above name.

Nothing more. Nothing less.

You can then hang a touchscreen over your head and display the above web page full screen.

Cost: 1 touch screen, glue. Protection helmet optional, depending on your trust in the glue.

You can consider web decks as live, interactive, responsive images. You can press buttons on the image, Cockpitdecks will adjust the image and its overlays to reflect X-Plane status.

Taxi safely.
