To add a new instruction to Cockpitdecks, the developer has to create a new ButtonInstruction sub-class derived from `cockpitdecks.ButtonInstruction` (or one of its subclass).

```python hl_lines="3-4"
from cockpitdecks.button import Button, ButtonInstruction


class SpecialInstruction(ButtonInstruction):

	INSTRUCTION_NAME = "button-special"

	def __init__(self, button: Button, config: dict):
		self._config = config

```

In the button definition:

```yaml hl_lines="3"
index: 42
name: ULTIMATE
type: push
command: button-special
```

# Example: Adding a ButtonInstruction to Print Information

The following python script adds a simple instruction to print some information

```python
import logging
from cockpitdecks.button import Button, ButtonInstruction

logger = logging.getLogger(__name__)
# logger.setLevel(logging.DEBUG)

class PrintInstruction(ButtonInstruction):
    """Custom instruction to print a message"""

    INSTRUCTION_NAME = "button-print"

    def __init__(self, name: str, button: Button, config: dict) -> None:
        ButtonInstruction.__init__(self, name=name, button=button)
        self._config = config
        self.message = config.get("message", "Hello, world!")

    def _execute(self):
        logger.info(f"{self.message}")
```

Its accompanying button definition:

```yaml hl_lines="4 6"
  - index: 1
    name: SAY HELLO
    label: SAY HELLO
    type: push
    command: button-print
    message: X-Plane rocks
```

When button is pressed, the message *X-Plane rocks* is printed on the console.

# Instruction API
