Cockpitdecks discovers about deck capabilities through a Deck Type structure.

A deck is presented to Cockpitdecks through a deck definition file called a Deck Type. The deck definition file describes the deck capabilities:

- How many buttons and how they can be manipulated,
- How many dials, if they can be turned, or pushed
- Feedback LCD screens for icons
- Feedback LED, optionally colored
- Ability to emit vibration or sound
- etc.

# Deck Definition

Here is for example, a deck configuration file for a Loupedeck LoupedeckLive device.

```yaml
# This is the description of a deck's capabilities for a Loupedeck LoupedeckLive device
#
---
name: LoupedeckLive
driver: loupedeck
buttons:
  - name: 0
    action: push
    feedback: image
    image: [90, 90, 0, 0]
    repeat: 12
  - name: left
    action: swipe
    feedback: image
    image: [60, 270, 0, 0]
  - name: right
    action: swipe
    feedback: image
    image: [60, 270, 420, 0]
  - name: 0
    prefix: e
    action: [encoder, push]
    feedback: none
    repeat: 6
  - name: 0
    prefix: b
    action: push
    feedback: colored-led
    repeat: 8
  - name: buzzer
    action: none
    feedback: vibrate
```

## Attributes

| Attribute  | Definition                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| name       | Name used inside Cockpitdecks to identifying the deck model.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| driver     | Keyword identifying the deck software driver class. (See main drivers class above.)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| background | The `background` attribute is an optional attribute only used by web decks. It specifies a background color and/or image to use for web deck representation. See explanation and exemple below.                                                                                                                                                                                                                                                                                                                                                                    |
| buttons    | The `Buttons` contains a list of *Deck Button Type Block* descriptions.<br><br>This attribute is named Buttons, with Button having the same meaning as in Cockpitdecks. A Deck Type Button is a generic term for all possible means of interaction on the deck:<br><br>1. Keys to press,<br>2. Encoders to turn,<br>3. Touchscreens to tap or swipe<br>4. Cursors to slide<br><br>A list of button types, each ButtonType leading to one or more individual buttons identified by their index, built from the `prefix`, `repeat`, and `name` attribute. See below. |

## Deck Type Button Block

A Deck Type Button Block defines either a single button, or a group of identical buttons. For example, if a deck has a special, unique, «Escape» button, it can be defined alone in a Deck Type Button Block. Similarly, if a deck consist of a grid of regularly spaced 6 by 4 keys that are all the same, they can also be defined in a single Deck Type Button Block.

### Attributes

| Attribute | Defintion                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name      | Name of the button type.<br><br>The name of the button type is<br><br>- either the final name of the button, like `touchscreen`, when there is a single button with that name on the deck,<br>- or an *integer value* that will be used to build the button names, in the case the block defines a sets of identical buttons.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| actions   | Interaction with the button. Interaction can be:<br><br>- `none`: There is no interaction with the button. It is only used for display purpose. (`none` interaction can be omitted.)<br>- `press`: Simple press button that only reports when it is pressed (one event)<br>- `push`: Press button that reports 2 events, when it is pushed, and when it is released. This allow for "long press" events.<br>- `swipe`: A surface swipe event, with a starting touch and a raise events.<br>- `encoder`: A rotating encoder, that can turn both clockwise and counter-clockwise<br>- `cursor`: A linear cursor (straight or circular) delivering values in a finite range.<br><br>Action can ba a single interaction or an array of interactions like `[encoder, push]` if a button combines both ways of interacting with it. |
| feedbacks | Feedback ability of the button. Feedback can be:<br><br>- `none`: No feedback on device, or direct feedback provided by some marks on the deck device. (`none` feedback can be omitted.)<br>- `image`: Small LCD iconic image.<br>- `led`: Simple On/Off LED light.<br>- `colored-led`: A single LED that can be colored.<br>- `multi-leds`: Several, single color, LED on a ramp.<br>- `encoder-leds`: Special encoder led ramp for X-Touch Mini (4 modes)<br>- `vibrate`: emit a buzzer sound.                                                                                                                                                                                                                                                                                                                              |
| repeat    | In case of a set if identical buttons, `repeat` if the number of time the same button is replicated along width (x) and height (y) axis.<br><br>If only one value is supplied, it is supposed to be `[value, 1]` array. For vertical layout, specify `[1, value]` instead.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |

### Action

An Action is a keyword that represent a physical mean of interaction with a device:

- `none`: There is no interaction with the button. It is only used for display purpose. (`none` interaction can be omitted.)
- `press`: Simple press button that only reports when it is pressed (one event)
- `push`: Press button that reports 2 events, when it is pushed, and when it is released. This allow for "long press" events.
- `swipe`: A surface swipe event, with a starting touch and a raise events.
- `encoder`: A rotating encoder, that can turn both clockwise and counter-clockwise
- `cursor`: A linear cursor (straight or circular) delivering values in a finite range.

### Feedback

A Feedback is a keyword that represent a physical mean for a deck to communicate some information (i.e. provide some feedback):

- `none`: No feedback on device, or direct feedback provided by some marks on the deck device. (`none` feedback can be omitted.)
- `image`: Small LCD iconic image.
- `led`: Simple On/Off LED light.
- `colored-led`: A single LED that can be colored.
- `multi-leds`: Several, single color, LED on a ramp.
- `encoder-leds`: Special encoder led ramp for X-Touch Mini (4 modes)
- `vibrate`: emit a buzzer

### Action and Feedback Usage

Action and Feedback keywords are used to express requirements in respectively Activation and Representation.

For example, an Activation will tell, by listing Actions, which types of interaction it requires.

```python hl_lines="5"
class LightDimmer(Activation):
    """Customized class to dim deck"""

    ACTIVATION_NAME = "dimmer"
    REQUIRED_DECK_ACTIONS = [DECK_ACTIONS.PRESS, DECK_ACTIONS.LONGPRESS, DECK_ACTIONS.PUSH]

    def __init__(self, config: dict, button: "Button"):
```

A deck button that can be used for the LightDimmer activation must have one of the listed action:

```yaml hl_lines="7"
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

```

Same principle applies to Representation and Feedback.

##### Examples of button names

In the case of a single button, the name of the button will be `touchscreen` and that name needs to be unique for the deck type.

```yaml
name: touchscreen
```

In the case of a set of identical buttons, the name of the button will be built from other attributes:

```yaml
name: 5
prefix: k
repeat: [4, 3]
```

Names of buttons will be: `k5`, `k6`, `k7`, ... `k16`.

##### Feedback Trick

> [!NOTE] Trick
> If a deck has a vibrate capability, it is advisable to declare it as a separate button of interaction, and use that button like any other. Vibrate is a feedback mechanism.

## Deck Type Button Block: Additional Attributes for Web Decks

The above Deck Type Button Block attributes are necessary for all decks, both physical and web decks. Web Decks also contain an additional series of attributes that drive the drawing of the deck in a web navigator window.

> [!NOTE] Web Deck Drawings
> For simplicity, Web Deck Drivers are drawn on an HTML Canvas, which is a pixel-driven drawing space. Web Decks are drawn with images and drawing instructions that use the pixel as a unit for display.

Web decks can have the following types of interactive buttons:

1. Keys (simple press, long press, etc.)
2. Encoders (turned clockwise, counter clockwise)
3. Touchscreen (pressed, swiped)
4. Slider (slid between 2 range values)

When rendered in a browser window, web deck interaction means are materialised through a changing pointer cursor (arrow, curved arrow (encode), single dot (push), double dot (pull), etc.)

> [!NOTE] Background Deck Type Attribute
> Please read above in this page the `background` Deck Type attribute used to specify a background image to use for web deck display.

### Attributes

| Attribute    | Definition                                                                                                                                                                                                                                                                                                                                                                 |
| ------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| dimension    | The dimension attribute can be a single integer value or a list of two values.<br><br>It determine the size of the button has drawn on the Web deck.<br><br>If the feedback visualisation is an `image`, the `image` attribute specifies the characteristics of the image. `X`is horizontal and correspond to the `width`, `Y` is vertical and correspond to the `height`. |
| layout       | Layout of the buttons on the web deck canvas.<br>- Offset<br>- Spacing<br>Buttons will be arranged at regular interval, starting from Offset, with supplied spacing between the buttons. Button sizes are specified in the Dimension attribute.                                                                                                                            |
| hardware     | Configuration information for a specific drawing representation of this hardware button.                                                                                                                                                                                                                                                                                   |
| empty-button | Minimal Button definition to create an empty, dummy button. This dummy button will becreated and used to generate an "empty" hardware representation (completely `off`). (In other words, if a button with hardware representation is not defined, this definition will be used to create a button.) See below.                                                            |
| options      | Comma-separated list of options, a single option can either be a name=value, or just a name, in which case the value is assumed to be True.<br><br>`options: count=8,active`<br><br>sets options `count`to value 8, and `active` to True. `active` is equivalent to `active=true`.                                                                                         |

## Deck  Type `background`

In addition to the above button definitions, a deck type may contain a `background` attribute. This attribute is only used by web decks. The background attribute defines the background of the web page where the web deck will be rendered. The background can either be

1. A PNG image,
2. A solid color and size information.
In the case of a PNG image, the size is deduced from the size of the image.

### Attributes

| Attribute | Definition                                                                                  |
| --------- | ------------------------------------------------------------------------------------------- |
| `image`   | Name of a PNG image, with extension. No default.                                            |
| `color`   | Color of the background of the web page. No default.                                        |
| `size`    | Array of two values with width and height of the web canvas. Defaults to (200, 100) pixels. |
| `overlay` | Static text or image overlay. Not implemented yet.                                          |

## Special Button « Hardware » Representation

Some deck buttons need a special representation or drawing to visually reproduce the physical deck equivalent button. Examples of such special representations are

- LoupedeckLive « colored » numbered buttons,
- X-Touch Mini LEDs around the encoders.

These highly specific « drawings » are performed on side representations called *Hardware Representations*.

Technically speaking, they behave very much like LCD representations:

- Some screen space is reserved on the canvas to host the representation,
- The hardware representation driver produces an image that mimics the hardware button on the send,
- Cockpitdecks « sends » the hardware representation image to the web deck for display in the reserved space,
- Like any other representation, the hardware representation gets updated each time the underlying values gets updated.
Hardware representation only exists for web decks to draw a very specific button.

### Empty Button Definition

Hardware Representations need to know how to represent themselves in case they are not used or defined on a page. To present nicely, Cockpitdecks needs to know how to draw an "undefined", unused Hardware Representation. To do this, Cockpitdecks uses a trick: It dynamically create an dummy placeholder button from the Empty Button Definition for its Hardware Representation. Basically, only two attributes need to be defined: A type (often set to none) and an attribute that tells its Hardware Representation. Optionally, a default, initial value can be supplied. Sometimes, some mandatory or optional Hardware Representation attributes need to be supplied as well. Here is an exemple of a simple LED representation.

```
hardware:
	type: virtual-xtm-led
	empty-button:
		type: none
		led: single
		initial-value: 1
```

![[hardware-representation-empty.png|400]]

# Examples of Deck Type Button Definition Block

#### Single Button Definition

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

#### Multiple Button Definition

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

# Installed Deck Types

On startup, Cockpitdecks reports which deck types are available:

```
Cockpitdecks 12.1.1.20241002 © 2022-2024 Pierre M
Elgato Stream Decks, Loupedeck decks, Berhinger X-Touch Mini, and web decks to X-Plane 12.1+

INFO  start.py:<module>:248: Initializing Cockpitdecks..
INFO  xplane.py:init:745: aircraft dataref is sim/aircraft/view/acf_livery_path
WARN  xplane.py:add_datarefs_to_monitor:1171: no connection
INFO  xplane.py:add_datetime_datarefs:803: monitoring simulator date/time datarefs
INFO  cockpit.py:add_extension_paths:337: added extension path /Users/pierre/Developer/fs/cockpitdecks_ext to sys.path
INFO  cockpit.py:add_extension_paths:342: loaded package cockpitdecks_ext and all its subpackages/modules (recursively)
INFO  cockpit.py:init:234: available simulator: X-Plane
INFO  cockpit.py:init:237: available deck drivers: xtouchmini, virtualdeck, loupedeck, streamdeck
INFO  cockpit.py:load_deck_types:999: loaded 20 deck types (Virtual Deck for Development, LoupedeckLive...), 12 are virtual deck types
INFO  cockpit.py:scan_devices:507: device drivers installed for xtouchmini 1.3.6, virtualdeck (included), loupedeck 1.4.5, streamdeck 0.9.5; scanning for decks and initializing them (this may take a few seconds)..
INFO  cockpit.py:scan_devices:532: found 1 xtouchmini
INFO  cockpit.py:scan_devices:532: found 1 loupedeck
INFO  cockpit.py:scan_devices:532: found 3 streamdeck
INFO  start.py:<module>:250: ..initialized
```
