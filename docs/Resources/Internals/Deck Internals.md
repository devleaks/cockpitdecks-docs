Deck Internals explains how user interactions on a physical deck device enter Cockpitdecks.

# How Deck User Interactions Enter Cockpitdecks

When a user want to use a deck with Cockpitdecks, it is necessary to have a python package available to interact with it.

By design and coïncidence, all three deck brands currently proceed with similar means.

The python package that interfaces the physical deck to the python language request to supply and install a callback function, with a proper interface.

The function, that we supply, is called each time an interaction occurs on the physical deck device.

When Cockpitdecks is started, its scans for available devices, and check whether the interfacing software package is available. If it is, it installs its own callback function into the python package for that deck. From that moment on, each time something occurs on the deck device, Cockpitdecks' callback function gets called.

In that callback function, Cockpitdecks tries to spend a minimum time. From the data it receives, it creates an [[Event]] with all necessary data and enqueues it for later processing by Cockpitdecks.

The Event contains information about the deck, of course, but also the precise button, knob, encoder, slider, screen... that was used and what type of interaction occurred (pushed, turned, swiped...)

# Deck Description

A deck is presented to Cockpitdecks through a deck definition file. The deck definition file describes the deck capabilities:
- How many buttons,
- How many dials, if they can be turned, or pushed
- Feedback screen icons
- Feedback LED
- Ability to emit vibration or sound

## Deck Defintion
Here is for example, a configuration file for a Loupedeck LoupedeckLive device.

```yaml
# This is the description of a deck's capabilities for a Loupedeck LoupedeckLive device
#
---
type: Stream Deck +
driver: streamdeck
buttons:
  - name: 0
    action: push
    feedback: image
    image: [96, 96, 0, 0]
    repeat: 8
  - name: 0
    prefix: e
    action: [encoder, push]
    feedback: none
    repeat: 4
  # touchscreen in streamdeck package vocabulary
  - name: touchscreen
    action: swipe
    feedback: image
    image: [800, 100, 0, 0]
```

## Definition Attributes

### Type

Keyword used for identifying the deck model.

### Driver

Keyword identifying the deck software driver class.

### Buttons

A list of button types, each ButtonType leading to one or more individual buttons identified by their index, built from the `prefix`, `repeat`, and `name` attribute. See below.
#### Name

Name of the button type.

The name of the button type can either be its real name, like `touchscreen` when there is a single button with that name, or an integer value to build another name from its `prefix` and (interger) name value.

Example

```yaml
name: touchscreen
```

In this case, the name of the button will be `touchscreen` and that name needs to be unique for the deck type.

```yaml
name: 5
prefix: k
repeat: 4
```

In the later case, names of buttons will be: `k5`, `k6`, `k7`, and `k8`.

#### Actions
Interaction with the button. Interaction can be:

- `none`: There is no interaction with the button. It is only used for display purpose.
- `press`: Simple press button that only reports when it is pressed (one event)
- `push`: Press button that reports 2 events, when it is pushed, and when it is released. This allow for "long press" events.
- `swipe`: A surface swipe event, with a starting touch and a raise events.
- `encoder`: A rotating encoder, that can turn both clockwise and counter-clockwise
- `cursor`: A linear cursor (straight or circular) delivering values in a finite range.

ACtion can ba a single interaction or an array of interactions like `[encore, push]` if a button combines both ways of interacting with it.

#### Feedback
Feedback ability of the button. Feedback can be:

- `none`: No feedback on device, or direct feedback provided by some marks on the deck device.
- `image`: Small LCD iconic image.
- `led`: Simple On/Off LED light.
- `colored-led`: A single LED that can be colored.
- `multi-leds`: Several, single color, LED on a ramp.
- `vibrate`: emit a buzzer sound.
- `sound`: emit a sound.


> [!NOTE] Trick
> If a deck has a vibrate capability, it is advisable to declare it as a separate button of interaction, and use that button like any other. Vibrate is a feedback mechanism.


#### Image
If the feedback visualisation is an `image`, the `image` attribute specifies the characteristics of the image (size, and eventually, offset position on a larger surface.) `X`is horizontal and correspond to the `width`, `Y` is vertical and correspond to the `height`.

## Deck Type

The above definition file is read by a Deck Type class.
The Deck Type class is responsible for providing information about the deck's capabilities, but also to control them. For example, given a button definition in Cockpitdecks, the Deck Type class can validate the button definition, ensuring that the button specified by its index is capable of the requested activation and representation.
# Event Processing

From the parameter supplied in the callback function, Cockpitdecks determine the type of interaction that occurred (pushed, turned, swiped...). For that interaction, an Event of a precise type is created, with all detailed parameters available to it. The callback function does not execute the activition but rather enqueues the event for later processing.

In Cockpitdecks, another 
# Activation

The activation is the piece of code that will process the event.

```python
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

# Representation

```python
class Icon(Representation):

    REPRESENTATION_NAME = "icon"
    REQUIRED_DECK_FEEDBACKS = DECK_FEEDBACK.IMAGE

    def __init__(self, config: dict, button: "Button"):
        Representation.__init__(self, config=config, button=button)

```

Similarly, when a Representation code is created, it must mention its identification keyword `REPRESENTATION_NAME` that will be searched in the button definition attribute.

The `REQUIRED_DECK_FEEDBACKS` determine which of the deck's definition `feedback` type is requested to be able to use the Representation().

# Button Definition

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

# Deck Driver (Hardware Interface)

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

- get_display_for_pil
- create_icon_for_key
- scale_icon_for_key
- send_key_image_to_device
- set_key_image

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
    def __init__(self, name: str,
                       config: dict,
                       cockpit: "Cockpit",
                       device = None):
    def init(self):
    def get_id(self):
    def read_definition(self):
    def get_button_value(self, name):
    def set_defaults(self, config: dict, base):
    def load_layout_config(self, fn):
    def inspect(self, what: str = None):
    def load(self):
    def change_page(self, page: str = None):
    def reload_page(self):
    def set_home_page(self):
    def load_home_page(self):
    def make_default_page(self, b: str = None):
    def get_index_prefix(self, index):
    def get_index_numeric(self, index):
    def valid_indices(self):
    def valid_activations(self, index = None):
    def valid_representations(self, index = None):
    def key_change_callback(self, deck, key, state):
    def key_change_processing(self, deck, key, state):
    def print_page(self, page: Page):
    def fill_empty(self, key):
    def render(self, button: Button):
    def start(self):
    def terminate(self):
```

For deck with iconic display capabilities:

```python
    def get_display_for_pil(self, b: str = None):
    def get_index_image_size(self, index):
    def load_icons(self):
    def get_icon_background(self, name: str, width: int, height: int,
          texture_in, color_in, use_texture = True, who: str = "Cockpit"):
    def create_icon_for_key(self, index, colors, texture, name: str = None):
    def scale_icon_for_key(self, index, image, name: str = None):
    def get_image_size(self, index):
    def _send_key_image_to_device(self, key, image):
```
