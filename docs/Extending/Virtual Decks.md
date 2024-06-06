In addition to real physical decks, Cockpitdecks offers virtual decks.

![[virtualdecks-text.png]]

There are two types of virtual decks:

1. Decks rendered on the graphical screen where cockpitdecks runs, called *Virtual Decks*.
2. Decks rendered in a internet navigator windows, aptly called *Web Decks*.

This is very handy to design layout for deck a developer does not necessary owns.

Virtual decks need to be declared at two places to be used by Cockpitdecks.

1. [[Deck Internals|Definition of the deck]], number of buttons, icon size, and layout of buttons. This allows Cockpitdecks to determine the virtual deck capabilities, like any other real physical deck.
2. Use of the deck, like any regular deck, with its Layout and is configuring parameters. To use a virtual deck it must appear in the list of decks in the `deckconfig/config.yaml` file.

Virtual decks communicate with Cockpitdecks through TCP/IP channels and Websockets for Web Decks. Each deck has its own (address, port) where Cockpitdecks sends updates (through a proxy WebSocket server for Web Decks). Similarly, each deck sends the interactions it detects in its window to Cockpitdecks at a given rendez-vous port.

# Virtual Deck Definition

Virtual decks are declared like any other deck. Their driver must be `virtualdeck`.

```yaml hl_lines="2 8"
type: Virtual Deck 3×2
driver: virtualdeck
buttons:
  - name: 0
    action: push
    feedback: image
    image: [256, 256]
    layout: [3, 2]
```

For virtual decks, the mandatory layout attribute fixes the number of keys horizontally, vertically. The equivalent `repeat` attribute is computed from it.

Image and repeat must match the deck layout. If any discrepancy is found an error is reported and the deck type ignored.

> [!NOTE] Virtual Decks
> For now, virtual decks only have a variable set of regular push button. Icons are restricted to square icons, Virtual decks may evolve in the future to emulate other features such as encoders, sliders, colored leds, led ramps, and touchscreens.

# Virtual Deck usage

To use a virtual deck for a given aircraft, it need to be declared in the list of decks available for that aircraft.

```yaml
# Definition of decks for Toliss A321
#
aicraft: Toliss 
decks:
  - name: Vdeck1
    type: Virtual Deck 3×2
    layout: devlayout
    address: 127.0.0.1
    port: 7701
```

Please note the specific attributes needed to address the virtual deck.

The same virtual deck definition can be used more than once to create similar decks:

```yaml 
decks:
  - name: Vdeck2
    type: Virtual Deck 3×2
    layout: test version
    address: 127.0.0.1
    port: 7702
```

If several virtual decks are running, it is necessary, like other decks, to specify its serial number in the serial.yaml file.

> [!WARNING] Serial Number of Virtual Decks
> The serial number of virtual decks is the address and tcp port name of the virtual deck.

```yaml 
Vdeck2: 127.0.0.1:7702
Vdeck1: 127.0.0.1:7701
```

## Address, Port

These attributes tell Cockpitdecks where it must send deck updates.

At creation time, the virtual deck will receive the (address, port) where Cockpitdecks listen to their interactions. By default, Cockpitdecks listen for virtual decks on port 7700 (on the host where it runs.) A single port listen to interactions of all virtual decks.

Virtual decks include their name in the messages they send to Cockpitdecks. This allows Cockpitdecks to identify the deck that sent the message.

Virtual Decks can run on another computer than the one running X-Plane and/or Cockpitdecks.

# Starting Virtual Decks

Virtual decks are declared in an aircraft `deckconfig/config.yaml`. It is therefore necessary to supply the aircraft currently being used to start virtual decks.

There is a utility script, very much like the one used to start Cockpitdecks itself, in the `bin` directory:

```sh
$ python bin/virtualdeck_start.py aircrafts/A321
```

To start web decks, it is advised to first start Cockpitdecks so that web decks can be discovered, then simply issue:

```sh
$ python bin/webdeck_start.py
```

Web Decks are automatically and dynamically discovered when Cockpitdecks runs. (The nature of coding of *pyglet* does unfortunately not allow a similar, dynamic mechanism for Virtual Decks.)

Recall, to start Cockpitdecks:

```sh
$ python bin/cockpitdecks_upd_start.py aircrafts/A321
```

Virtual Decks and Web Decks are started in their own processes that does not interfere with Cockpitdecks. In both case, a global application loop run to handle interactions.

For Virtual Decks, piglet main loop runs:

```python
VirtualDeckManagerUI.run(interval=0.8)  # seconds
```

For Web Decks, Flask app runs:

```python
app = Flask(__name__, template_folder=TEMPLATE_FOLDER)
...
app.run(host=APP_HOST[0], port=APP_HOST[1])	
```

# Virtual Decks Internals

## Virtual Decks

*Virtual Decks* are developed thanks to the [pyglet](https://pyglet.readthedocs.io/en/latest/) python user interface package.

This package runs thanks to a loop the capture events on windows it manages. Usuallly, this loop runs as often as 60 times a second. There is not such need of fast interaction with virtual decks. Cockpitdecks asks the loop to run no more than 2 to 4 times a second. This can be adjusted in the virtual deck manager.

![[virtual decks.svg|600]]

```python
	VirtualDeckManagerUI.run(interval=0.8)  # seconds
```

## Web Decks

*Web Decks* are designed with simple standard web features, are rendered on an HTML Canvas, uses standard events to report interaction through basic JavaScript functions.
A Proxy application is necessary between Cockpitdecks and the browser to convert TCP/IP socket requests into WebSocket requests that can be understood by browsers.
The application that serves them is a very simple Flask application (2 routes) with 2 simple Ninja2 templates. The Flask application also runs the WebSocket proxy.

![[webdecks.svg|600]]

See [[Deck Internals#Folder Organisation]].

### Web Deck List (Web Deck Home Page)

![[webdeck-list.png]]

### Web Deck Example (Including Background Image)

![[webdeck-example.png]]
