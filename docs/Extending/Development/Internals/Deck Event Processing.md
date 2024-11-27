From the parameter supplied in the callback function, Cockpitdecks determine the type of interaction that occurred (pushed, turned, swiped...). For that interaction, an Event of a precise type is created, with all detailed parameters available to it. The callback function does not execute the activation but rather enqueues the event for later processing.

In Cockpitdecks, another thread of execution reads events from the queue and perform the required action. This cleanly separate event collection and event "execution" in two separate process threads.

# Activation

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

# Representation

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

