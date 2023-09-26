Cockpitdecks is designed to accommodate different deck models.
To allow Cockpitdeck to manage a new deck model, two interfaces need to be provided.

# Deck Definition
All buttons available for interaction are listed in a configuration file that enumerates all buttons, their interaction and display capabilities.
Here is a configuration file for a Loupedeck Loupedecklive device.

```yaml
# This is the description of a deck's capabilities for a Loupedeck LoupedeckLive device
#
---
name: loupedeck
model: loupedecklive
buttons:
    - name: 0
      action: push
      view: image
      image: [90, 90, 0, 0]
      repeat: 12
    - name: left
      action: swipe
      view: image
      image: [60, 270, 0, 0]
    - name: right
      action: swipe
      view: image
      image: [60, 270, 420, 0]
    - name: center
      action: swipe
      view: image
      image: [360, 270, 60, 0]
    - name: 0
      prefix: e
      action: encoder-push
      view: none
      repeat: 6
    - name: 0
      prefix: b
      action: push
      view: colored-led
      repeat: 8
```

## Configuration Attributes

### Name
Name of the button.

### Action
Interaction with the button. Interaction can be:
- `none`: There is no interaction with the button. It is only used for display purpose.
- `press`: Simple press button that only reports when it is pressed (one event)
- `push`: Press button that reports 2 events, when it is pushed, and when it is released. This allow for "long press" events.
- `swipe`: A surface swipe event, with a starting touch and a raise events.
- `encoder`: A rotating encoder, that can turn both clockwise and counter-clockwise
- `encoder-push`: An encoder couple to a push button.
- `cursor`: A linear cursor (straight or circular) delivering values in a finite range.

### View
Feedback ability of the button. Feedback visualisation can be:
- none: No feedback on device, or direct feedback provided by some marks on the deck device.
- image: Small LCD iconic image.
- led: Simple On/Off LED light.
- multi-leds: Several, single color, LED on a ramp.
- colored-led: A single LED that can be colored.

### Image
If the feedback visuallisation is an iconic image, the `image` attribute specifies the characteristics of the image (size, and eventually, position on a larger surface.)

## Deck Defintion Result
The result of a deck definition is the list of valid definitions for each button of that deck. This includes, for each button, its activation capabilities, its representation capabilties, and the list of valid index name to designate a precise button on the deck.
Here is an excerpt of the meta data available in the definition of a button:

```yaml
...
'2':
  index: '2'
  _index: 2
  action: push
  view: image
  activations:
  - page
  - reload
  - inspect
  - stop
  - push
  - longpress
  - onoff
  - updown
  representations:
  - icon
  - text
  - icon-color
  - multi-icons
  - multi-texts
  - icon-animate
  - side
  - data
  - annunciator
  - annunciator-animate
  - switch
  - circular-switch
  - push-switch
  - ftg
  - weather
  image: [90, 90, 0, 0]
...
e0:
  index: e0
  _index: 0
  action: encoder-push
  view: none
  activations:
  - page
  - reload
  - inspect
  - stop
  - push
  - longpress
  - onoff
  - updown
  - encoder
  - encoder-push
  - encoder-onoff
  - encoder-value
  - knob
  representations:
  - icon
  - text
  - icon-color
  - multi-icons
  - multi-texts
  - icon-animate
  - side
  - data
  - annunciator
  - annunciator-animate
  - switch
  - circular-switch
  - push-switch
  - ftg
  - weather
...
slider:
  index: slider
  _index: 0
  action: slider
  view: none
  activations:
  - slider
  representations:
  - none
...
```

The meta data is available in each button.

# Deck Hardware Interface
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
**Feedback**:
*If the deck has image capabilities*:
- get_display_for_pil
- create_icon_for_key
- scale_icon_for_key
- send_key_image_to_device
- set_key_image
*If the deck has virating capabilities*:
- vibrate
*If the deck has button color capabilties*:
- set_button_color

The following functions are also necessary and can be overwritten if necessary.

```python
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
    def __init__(self, name: str, config: dict,
                       cockpit: "Cockpit", device = None):
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

