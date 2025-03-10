# Core Button

The core [[Button]] class is designed to isolate deck key specifics in two attribute classes:

1. Activation, that represents the user's interaction with the deck, and
2. Representation, that expresses what the deck communicates back to the user.

In addition to these two attributes, the core Button class holds, keeps and maintains a set of other global, generic attributes made available to the two governing attributes.

- All datarefs accessed by the button,
- Guard, if any,
- Whether the button is "managed" and displays alternate representation,

Last but not least, the button keeps a copy of its entire "definition", a dictionary of name, value pairs supplied through the configuration file of the Page. Again, some attributes are mandatory and/or imposed by Cockpitdecks, like

- `type`
- `index`
- `options`
- `set-dataref`
- ...

Other attributes may already be defined by existing activations and representations, but any arbitrary name, value pair can be passed and stored in the button configuration dictionary and used by its activation and/or its representation.

# New Representation

Creating a new representation, like sending some special information or custom image to a LCD key is more probable.

To add a new representation to Cockpitdecks, the developer has to create a new Representation sub-class and derived it from `cockpitdecks.buttons.representation.Representation` or one of its subclass.

```python hl_lines="3-4"
class SpecialtyIcon(Representation):

    REPRESENTATION_NAME = "specialty-icon"
    REQUIRED_DECK_FEEDBACKS = DECK_FEEDBACK.IMAGE

    def __init__(self, config: dict, button: "Button"):
        Representation.__init__(self, config=config, button=button)

        self.param1_value = config.get("param1")

```

In the button definition:

```yaml hl_lines="4"
index: 42
name: SPECIAL_ICON
type: push
specialty-icon:
	param1: value
```

> [!NOTE] Abstract Representation.
> It is sometimes convenient to create a *base* class from which derived classes inherit. If the (abstract) base class cannot be instanciated, terminate its internal name with `-base`. By convention, activation and representation classes with internal names that terminates with `-base` are excluded from instanciable classes.

```yaml
		REPRESENTATION_NAME = "do-not-instanciate-base"
```

# Representation API
