To add a new simulator to Cockpitdecks, the developer has to create a new Simulator sub-class derived from `cockpitdecks.Simulator` (or one of its subclass).

```python hl_lines="3-4"
class SubLogicFlightSimulator(Simulator):

    name = "A2FS1"

    def __init__(self, cockpit, environ):

```

# Simulator API
