Cockpitdecks is designed to accommodate different deck types.
To allow Cockpitdecks to manage a new deck type, two interfaces need to be provided.

## Glossary

A Deck is a physical device. It has 

- a Manufacturer (`brand`))
- a Model name (`model`)
- a Serial number, supplied in the `secret.yaml` file.

Inside Cockpitdecks, a Deck has the following attributes:

- A **Type**, which is an internal name for a deck "model". The **Type** of a Deck explicitely describes the capabilities of the Deck (number of buttons, display capabilities, etc.)
- A **Driver**, which is a link to the software that make the "bridge" between Cockpitdecks and the device connected to the computer.


# Deck Types
All buttons available for interaction are listed in a configuration file that enumerates all buttons, their possible interaction and display capabilities.
Here is a configuration file for a Loupedeck LoupedeckLive device.

```yaml
# This is the description of a deck's capabilities for a Loupedeck LoupedeckLive device
#
---
type: loupedecklive
brand: Loupedeck
model: loupedecklive
driver: loupedeck
buttons:
    - name: 0
      activation: push
      representation: image
      image: [90, 90, 0, 0]
      repeat: 12
    - name: left
      activation: swipe
      representation: image
      image: [60, 270, 0, 0]
    - name: right
      activation: swipe
      representation: image
      image: [60, 270, 420, 0]
    - name: center
      activation: swipe
      representation: image
      image: [360, 270, 60, 0]
    - name: 0
      prefix: e
      activation: [encoder, push]
      representation: none
      repeat: 6
    - name: 0
      prefix: b
      action: push
      representation: colored-led
      repeat: 8
```

> [!NOTE]
> In this very special case, there will be a conflict because a portion of the screen is used twice: The `center` screen correspond to the 12 push buttons. Deck should use either one but not both. If both remain activated, Cockpitdecks will warn about illegal use of center screen for button or the opposite. Example:
> `set_key_image: key «center»: invalid index for center display, aborting set_key_image`
> These warnings can safely be ignored. To suppress the warning, simply comment out the portion of unused buttons.

## Configuration Attributes

### Model

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
- `encoder-push`: An encoder and a push button combined in one feature. The button can be pushed, turned, released, etc. all *simultaneously*.
- `cursor`: A linear cursor (straight or circular) delivering values in a finite range.

#### Feedback
Feedback ability of the button. Feedback can be:

- `none`: No feedback on device, or direct feedback provided by some marks on the deck device.
- `image`: Small LCD iconic image.
- `led`: Simple On/Off LED light.
- `colored-led`: A single LED that can be colored.
- `multi-leds`: Several, single color, LED on a ramp.
- `vibrate`: emit a buzzer sound.
- `sound`: emit a sound.

#### Image
If the feedback visualisation is an `image`, the `image` attribute specifies the characteristics of the image (size, and eventually, offset position on a larger surface.) `X`is horizontal and correspond to the `width`, `Y` is vertical and correspond to the `height`.

## Deck Type

The result of a deck definition is the list of valid definitions for each button of that deck. This includes, for each button, its activation capabilities, its representation capabilities, and the list of valid index name to designate a precise button on the deck.
The meta data is available in each individual button in the `_defs` attribute

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

(in more recent versions of drivers, there sometimes is a callback function per interaction type: key_change_call_back, dial_change_callback, touch_callback...)

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

# Other Simulator Software

Cockpitdecks software has been organized in such a way that it could probably be used with other simulator software, provided some adaptation of course. Adaptation would be confined to the interaction of Cockpitdecks with the simulator software.

Interaction from Cockpitdeck to simulator software:

1. Execute command (without parameter)
2. Change parameter (dataref) value
3. Read parameter (dataref) value (at up to ~2 to 5 Hz frequency)


# Algorithm for Deck Definition

(wip)

## Goal

When parsing a button definitions, determine its activation and its representation.

Given the button index, determine from the deck's definition for that index what the button is capable of (action, feedback)

Check whether the button is capable of the activation in its definition from the action capabilities

Check whether the button is capable of the representation in its definition from its feedback capabilities

Deck capabilities is announced in terms of individual buttons, and for each button:

action: what it can do, see above
feedback (formely views): what it can show/express as feedback, see above also.

## Algorithm

### Parsing Deck Definition

For each button,

Determine the actions the button is capable of (there can be more than one, e.g. push and encode)

Determine the feedback type (image, led, sound...). Ideally, there might be more than one.

Q: If a deck is capable of emitting sound, it is a the deck level, not the button level, so sound should be treated at the deck level.