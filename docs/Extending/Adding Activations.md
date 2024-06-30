To add a new activation to Cockpitdecks, the developer has to create a new Activation sub-class and declare it in the

`<cockpitdecks-source-code>/cockpitdecks/buttons/activation/__init__.py`

file.

```python hl_lines="3-4"
class SpecialAction(Activation):

    ACTIVATION_NAME = "special-action"
    REQUIRED_DECK_ACTIONS = [DECK_ACTIONS.PRESS, DECK_ACTIONS.LONGPRESS, DECK_ACTIONS.PUSH]

    def __init__(self, config: dict, button: "Button"):

```

In the button definition:

```yaml hl_lines="3"
index: 42
name: ULTIMATE
type: special-action
```


# Example: Adding an Activation to Dim Deck Backlight

The following python script adds a simple activation to adjust 

```python
import logging
from cockpitdecks import DECK_ACTIONS
from .activation import UpDown

logger = logging.getLogger(__name__)
# logger.setLevel(logging.DEBUG)

class LightDimmer(UpDown):
    """Customized class to dim deck back lights according to up-down switch value"""

    ACTIVATION_NAME = "dimmer"
    REQUIRED_DECK_ACTIONS = [DECK_ACTIONS.PRESS, DECK_ACTIONS.LONGPRESS, DECK_ACTIONS.PUSH]

    def __init__(self, config: dict, button: "Button"):
        UpDown.__init__(self, config=config, button=button)
        self.dimmer = config.get("dimmer", [10, 90])

    def activate(self, event):
        currval = self.stop_current_value
        if currval is not None and 0 <= currval < len(self.dimmer):
            self.button.deck.set_brightness(self.dimmer[currval])
        super().activate(event)
```

Its accompanying button definition:

```yaml hl_lines="4 6"
  - index: 1
    name: ANNUNCIATOR LIGHTS
    label: ANN LT
    type: dimmer
    stops: 3
    dimmer: [100, 80, 30]
    switch:
      switch-style: rect
      button-fill-color: black
      button-underline-width: 4
      button-underline-color: coral
      tick-labels:
        - "TEST"
        - "BRT"
        - "DIM"
      tick-space: 10
      tick-label-size: 30
      tick-label-font: DIN Bold
    options: 3way,invert,hexa
    dataref: AirbusFBW/AnnunMode
    set-dataref: AirbusFBW/AnnunMode
```

When switch is moved to TEST, backlight is set to 100%, BTR sets it to 80%, and DIM to 30%.