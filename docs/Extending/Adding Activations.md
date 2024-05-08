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
