Deck Internals explains how Cockpitdecks discovers about miscellaneous deck hardware capabilities and how user interactions on a physical deck device enter Cockpitdecks.

> [!WARNING] Work In Progress
> This important file is under DEEP re-writing.

# How Deck User Interactions Enter Cockpitdecks

When a user want to use a deck with Cockpitdecks, it is necessary to have a python package available to interact with it. This python package is not provided by Cockpitdecks but by other developers how bridged the physical deck hardware with the python language.

By design and coïncidence, all three deck brands currently proceed with similar means.

The python package that interfaces the physical deck to the python language request to supply and install a callback function, with a proper interface.

The function, that we supply, is called each time an interaction occurs on the physical deck device.

When Cockpitdecks is started, its scans for available devices, and check whether the interfacing software package is available. If it is, it installs its own callback function into the python package for that deck. From that moment on, each time something occurs on the physical deck device, Cockpitdecks' callback function gets called.

In that callback function, Cockpitdecks tries to spend a minimum time. From the data it receives, it creates an [[Events|Event]] with all necessary data and enqueues it for later processing by Cockpitdecks.

The Event contains information about the deck, of course, but also the precise button, knob, encoder, slider, screen... that was used and what type of interaction occurred (pushed, turned, swiped...) All that information is in the Event and the Event is sent to Cockpitdecks. That's how physical deck interaction enters Cockpitdecks.

Cockpitdecks runs a specific routine that listen to events that enter through the queue. Cockpitdecks instruct the event to *run*. That's how and when actions are actually performed, like sending a command to X-Plane or changing the value of a dataref.

In the overall software organisation of Cockpitdecks, everything related to decks is confined in a `decks` folder.

# Decks Folder Organisation

The `decks` folder is organized as follow: (between parenthesis, the content of the folder or file.)

```
deckconfig
  ⊢ resources
    ⊢ decks
decks
  ⊢ resources (accessory files)
    ⊢ templates (Jinja2 templates)
      ⊢ index.j2
      ⊢ deck.j2
    ⊢ assets (Web Deck assets)
      ⊢ decks (Web Deck specific assets)
        ⊢ images (Web Deck background images)
      ⊢ images (Web Applicatioin images and icons)
      ⊢ js (Interaction and display software in JavaScript)
    decktype.py (Deck Type class)
    ...
    deckdefinition.yaml (Deck types, one file per type)
    ...
    webdeckdefinition.yaml (Deck types, one file per type)
    ...
    virtualdeck.py (Virtual Deck implementation class)
    virtualdeckmanager.py (Virtual Deck Manager implementation class)
    ...
  loupedeck.py (main drivers for each type of deck)
  ...
  virtualdeck.py (main drivers for all virtual decks)
  xtouchmini.py (main drivers for each type of deck)
```

# Deck Type

Cockpitdecks discovers about deck capabilities through a Deck Type structure.

A deck is presented to Cockpitdecks through a deck definition file called a Deck Type. The deck definition file describes the deck capabilities:

- How many buttons,
- How many dials, if they can be turned, or pushed
- Feedback screen icons
- Feedback LED
- Ability to emit vibration or sound

## Installed Deck Types

On startup, Cockpitdecks will report which deck types are available:

```
Cockpitdecks 11.20.1 © 2022-2024 Pierre M <pierre@devleaks.be>
Elgato Stream Decks, LoupedeckLive, Berhinger X-Touch, and web decks to X-Plane 12

INFO MainThread start.py:<module>:100: Initializing Cockpitdecks..
INFO MainThread xplane.py:init:329: aircraft dataref is sim/aircraft/view/acf_livery_path
WARNING MainThread xplane.py:add_datarefs_to_monitor:727: no connection
INFO MainThread xplane.py:add_datetime_datarefs:378: monitoring simulator date/time datarefs
INFO MainThread cockpit.py:load_deck_types:808: loaded 20 deck types (Virtual Deck for Development, virtual loupedeck.live.s, Virtual Streamdeck Mini, Virtual Streamdeck MK.2, Virtual Streamdeck +, Virtual LoupedeckLive, Stream Deck XL, Stream Deck Neo, Virtual Streamdeck XL, Virtual Stream Deck Neo, LoupedeckLive, Streamdeck, Virtual X-Touch Mini, Stream Deck Original, Stream Deck Mini, virtual loupedeck.ct, Virtual LoupedeckLive with Mosaic, X-Touch Mini, Stream Deck +, Virtual XKeys 80), 12 are virtual deck types
INFO MainThread cockpit.py:scan_devices:357: drivers installed for streamdeck 0.9.5, loupedeck 1.4.5, xtouchmini 1.3.6; scanning for decks and initializing them (this may take a few seconds)..
INFO MainThread cockpit.py:scan_devices:367: found 3 streamdeck
INFO MainThread cockpit.py:scan_devices:367: found 1 loupedeck
INFO MainThread cockpit.py:scan_devices:367: found 1 xtouchmini
INFO MainThread start.py:<module>:102: ..initialized
```

## Deck Definition

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
| background | The `background` attribute is an optional attribute only used by web decks. It specifies a background color and/or image to use for web deck representation. See exemple below.                                                                                                                                                                                                                                                                                                                                                                                    |
| buttons    | The `Buttons` contains a list of *Deck Button Type Block* descriptions.<br><br>This attribute is named Buttons, with Button having the same meaning as in Cockpitdecks. A Deck Type Button is a generic term for all possible means of interaction on the deck:<br><br>1. Keys to press,<br>2. Encoders to turn,<br>3. Touchscreens to tap or swipe<br>4. Cursors to slide<br><br>A list of button types, each ButtonType leading to one or more individual buttons identified by their index, built from the `prefix`, `repeat`, and `name` attribute. See below. |

### Deck Type Button Block

A Deck Type Button Block defines either a single button, or a group of identical buttons. For example, if a deck has a special, unique, «Escape» button, it can be defined alone in a Deck Type Button Block. Similarly, if a deck consist of a grid of regularly spaced 6 by 4 keys that are all the same, they can also be defined in a single Deck Type Button Block.

#### Attributes

| Attribute | Defintion                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name      | Name of the button type.<br><br>The name of the button type is<br><br>- either the final name of the button, like `touchscreen`, when there is a single button with that name on the deck,<br>- or an *integer value* that will be used to build the button names, in the case the block defines a sets of identical buttons.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| actions   | Interaction with the button. Interaction can be:<br><br>- `none`: There is no interaction with the button. It is only used for display purpose. (`none` interaction can be omitted.)<br>- `press`: Simple press button that only reports when it is pressed (one event)<br>- `push`: Press button that reports 2 events, when it is pushed, and when it is released. This allow for "long press" events.<br>- `swipe`: A surface swipe event, with a starting touch and a raise events.<br>- `encoder`: A rotating encoder, that can turn both clockwise and counter-clockwise<br>- `cursor`: A linear cursor (straight or circular) delivering values in a finite range.<br><br>Action can ba a single interaction or an array of interactions like `[encoder, push]` if a button combines both ways of interacting with it. |
| feedbacks | Feedback ability of the button. Feedback can be:<br><br>- `none`: No feedback on device, or direct feedback provided by some marks on the deck device. (`none` feedback can be omitted.)<br>- `image`: Small LCD iconic image.<br>- `led`: Simple On/Off LED light.<br>- `colored-led`: A single LED that can be colored.<br>- `multi-leds`: Several, single color, LED on a ramp.<br>- `encoder-leds`: Special encoder led ramp for X-Touch Mini (4 modes)<br>- `vibrate`: emit a buzzer sound.                                                                                                                                                                                                                                                                                                                              |
| repeat    | In case of a set if identical buttons, `repeat` if the number of time the same button is replicated along width (x) and height (y) axis.<br><br>If only one value is supplied, it is supposed to be `[value, 1]` array. For vertical layout, specify `[1, value]` instead.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |

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

### Deck Type Button Block: Additional Attributes for Web Decks

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

#### Attributes

| Attribute | Definition                                                                                                                                                                                                                                                                                                                                                                 |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Dimension | The dimension attribute can be a single integer value or a list of two values.<br><br>It determine the size of the button has drawn on the Web deck.<br><br>If the feedback visualisation is an `image`, the `image` attribute specifies the characteristics of the image. `X`is horizontal and correspond to the `width`, `Y` is vertical and correspond to the `height`. |
| layout    | Layout of the buttons on the web deck canvas.<br>- Offset<br>- Spacing<br>Buttons will be arranged at regular interval, starting from Offset, with supplied spacing between the buttons. Button sizes are specified in the Dimension attribute.                                                                                                                            |
| hardware  | Configuration information for a specific drawing representation of this hardware button.                                                                                                                                                                                                                                                                                   |
| options   | Comma-separated list of options, a single option can either be a name=value, or just a name, in which case the value is assumed to be True.<br><br>`options: count=8,active`<br><br>sets options `count`to value 8, and `active` to True. `active` is equivalent to `active=true`.                                                                                         |

### Special Button Representation

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

### Examples of Deck Type Button Definition Block

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

# Deck Event Processing

From the parameter supplied in the callback function, Cockpitdecks determine the type of interaction that occurred (pushed, turned, swiped...). For that interaction, an Event of a precise type is created, with all detailed parameters available to it. The callback function does not execute the activation but rather enqueues the event for later processing.

In Cockpitdecks, another thread of execution reads events from the queue and perform the required action. This cleanly separate event collection and event "execution" in two separate process threads.

## Activation

The activation is the piece of code that will process the event.

```python hl_lines="6"
class Push(Activation):
    """
    Defines a Push activation.
    The supplied command is executed each time a button is pressed.
    """
    ACTIVATION_NAME = "push"
    REQUIRED_DECK_ACTIONS = DECK_ACTIONS.PUSH
```

Activation usually leads to either

- one or more command sent to the simulator for execution
- internal changes of the deck, like loading a new page of buttons
- or both

## Representation

```python hl_lines="6"
class Annunciator(DrawBase):
    """
    All annunciators are based on this class.
    See docs for subtypes and models. 
    """
    REPRESENTATION_NAME = "annunciator"
    def __init__(self, config: dict, button: "Button"):
        self.button = button
```

Similarly, when a Representation code is created, it must mention its identification keyword `REPRESENTATION_NAME` that will be searched in the button definition attribute.

The `REQUIRED_DECK_FEEDBACKS` determine which of the deck's definition `feedback` type is requested to be able to use the Representation().

## Button Definition

The `ACTIVATION_NAME` is the string that the button definition must use to trigger that activation (`type` attribute):

```yaml
  - index: 1
    name: MASTER CAUTION
    type: push
    command: sim/annunciator/clear_master_caution
    annunciator:
      text: "MASTER\nCAUT"
      text-color: darkorange
      text-font: DIN Condensed Black.otf
      text-size: 72
      dataref: AirbusFBW/MasterCaut
    vibrate: RUMBLE5
```

## Deck Driver (Hardware Interface)

The second interface provided for decks is the software necessary to:

1. Collect interactions from the device (which button has been pressed, how long?, which encoder has been turned, how fast?)
2. Send feedback visualisation instruction to the device to reflect the state change.

This require the coding of a bridge between Cockpitdecks and the API that controls the device.

Currently, this require the coding of a single python class with the following functions:

**General**:

- make_default_page
- render

**Interaction control**:

- key_change_callback

In some drivers, there sometimes is a callback function per interaction type:

- key_change_call_back,
- dial_change_callback
- touch_callback...

**Feedback**:

*If the deck has image capabilities*:

See list below.

*If the deck has sound and/or vibrating capabilities*:

- vibrate

*If the deck has lit button with color capabilities*:

- set_button_color

*If the deck has lit button without color capabilities*:

- set_button_led

*If the deck has lit button with several led capabilities* (led ramps, etc.):

- set_button_encoder_led

The following functions are also necessary and can be overwritten if necessary.

```python
    def __init__(self, name: str, config: dict, cockpit: "Cockpit", device=None)
    def set_deck_type(self)

	def init(self)
    def get_id(self) -> str:

	def is_virtual_deck(self) -> bool:
    def get_deck_button_definition(self, idx)
    def get_deck_type(self) -> DeckType:
    def get_attribute(self, attribute: str, silence: bool = False)
    def load(self)

	def change_page(self, page: str | None = None)
    def reload_page(self)
    def set_home_page(self)
    def load_home_page(self)
    def make_default_page(self, b: str | None = None)

	def get_button_value(self, name)

	def get_index_prefix(self, index)
    def get_index_numeric(self, index)
    def valid_indices(self, with_icon: bool = False)
    def valid_activations(self, index=None)
    def valid_representations(self, index=None)

	def inspect(self, what: str | None = None)
    def print_page(self, page: Page)

    def vibrate(self, button)
    def set_brightness(self, brightness: int)

	def render(self, button: Button)
	def fill_empty(self, key)
    def clean_empty(self, key)

	def start(self)
    def terminate(self)
```

For deck with iconic display capabilities:

```python
    def get_image_size(self, index)
    def create_empty_icon_for_key(self, index)
    def get_icon_background(
        self,
        name: str,
        width: int,
        height: int,
        texture_in,
        color_in,
        use_texture=True,
        who: str = "Deck",
    ):
    def create_icon_for_key(self, index, colors, texture)
    def scale_icon_for_key(self, index, image, name: str | None = None)
    def fill_empty(self, key)
    def clean_empty(self, key)
    def set_key_icon(self, key, image)
```

## Installed Deck Drivers

On startup, Cockpitdecks will report which drivers are installed:

```
INFO: drivers installed for streamdeck 0.9.5, loupedeck 1.4.5, xtouchmini 1.3.6; scanning..
INFO: found 3 streamdeck
INFO: found 1 loupedeck
INFO: found 1 xtouchmini
```

# Web Decks Internals

*Web Decks* are designed with simple standard web features, are rendered on an HTML Canvas, uses standard events to report interaction through basic JavaScript functions.
The application that serves them is a very simple Flask application with Ninja2 templates. The Flask application also runs the WebSocket proxy.

![[webdecks.svg|600]]

## Web Deck Messages

Meta data is a python dictionary serialized into JSON (mostly name, value pairs).

It can contain any arbitrary serialisable items.

### Cockpitdecks to Web Deck Messages

#### Send code

```js
{
    "code": code,
    "deck": name,
    "meta": {
        "ts": datetime.now().timestamp()
    }
}
```

#### Send Image

It can be a key image, or a «hardware» image.

```js
{
    "code": code,
    "deck": name,
    "key": key,
    "image": base64.encodebytes(content).decode("ascii"),
    "meta": {
        "ts": datetime.now().timestamp()
    }
}
```

### Web Decks to Cockpitdecks Messages

#### Send code

```js
{
    "code": code,
    "deck": name,
}
```

#### Send Event

```js
{
	"code": 0,
	"deck": deck,
	"key": key,
	"event": value,
	"data": data
}
```
