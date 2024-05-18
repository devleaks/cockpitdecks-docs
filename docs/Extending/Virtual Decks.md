In addition to real physical deck, Cockpitdecks offers simple decks rendered on the screen where cockpitdecks runs.

This can be very handy to design layout for deck a developer does not necessary owns.

Virtual decks need to be declared at two places to be used by Cockpitdecks.

1. Definition of the deck, number of buttons, icon size, and layout of buttons.
2. Use of the deck, like any regular deck, with its Layout and is configuring parameters. To use a virtual deck it must appear in the list of decks in the `deckconfig/config.yaml` file.

Virtual decks communicate with Cockpitdecks through TCP/IP channels. Each deck has its own (address, port) where Cockpitdecks sends updates. Similarly, each deck sends the interactions it detects in its window to Cockpitdecks at a given port.

# Virtual Deck Definition

Virtual decks are declared like any other deck. Their driver must be the keyword `virtualdeck`.

```yaml hl_lines="2"
type: Virtual Deck Type Name
driver: virtualdeck
layout: [3, 2, 256]
buttons:
  - name: 0
    action: push
    feedback: image
    image: [256, 256]
    repeat: 6
```

The mandatory layout attribute fixes the number of keys horizontally, vertically, and their size.

Image and repeat must match the deck layout. If any discrepancy is found an error is reported and the deck type ignored.

> [!NOTE] Virtual Decks
> For now, virtual decks only have a variable set of regular push button. Virtual decks may evolve in the future to emulate other features such as encoders, leds, and touchscreens.

# Virtual Deck usage

To use a virtual deck for a given aircraft, it need to be declared in the list of decks available for that aircraft.

```yaml
# Definition of decks for Toliss A321
#
aicraft: Toliss 
decks:
  - name: Vdeck1
    type: Virtual Deck Type Name
    layout: devlayout
    address: 127.0.0.1
    port: 7701
```

Please note the specific attributes needed to address the virtual deck.

The same virtual deck definition can be used more than once to create similar decks:

```yaml 
decks:
  - name: Vdeck2
    type: Virtual Deck Type Name
    layout: test version
    address: 127.0.0.1
    port: 7702
```

If several virtual decks are running, it is necessary, like other decks, to specify its serial number in the serial.yaml file.

> [!WARNING] Serial Number of Virtual Decks
> The serial number of virtual decks is the address and tcp port name of the virtual deck.

```yaml 
Vdeck2: 127.0.0.1:7702
```

## Address, Port

These attributes tell Cockpitdecks where it must send deck updates.

At creation time, the virtual deck will receive the (address, port) where Cockpitdecks listen to their interactions. By default, Cockpitdecks listen for virtual decks on port 7700 (on the host where it runs.) A single port listen to all interactions of all virtual decks.

In the message decks send to Cockpitdecks, they mention their name, which allows Cockpitdecks to identify the deck that sent the message.

# Starting Virtual Decks

Virtual decks are declared in an aircraft deckconfig/config.yaml. It is therefore necessary to supply the aircraft currently being used to start virtual decks.

There is a utility script, very much like the one used to start Cockpitdecks itself, in the `bin` directory:

```sh
$ python bin/virtualdeck_start.py aircrafts/A321
```

Recall, to start Cockpitdecks:

```sh
$ python bin/cockpitdecks_upd_start.py aircrafts/A321
```

# Virtual Decks Internals

Virtual decks is develop thanks to the [pyglet](https://pyglet.readthedocs.io/en/latest/) python user interface package.
