A *button* is the general term for a key, knob, rotary encoder, slider cursor, or even touch surface on a deck. On a given deck, each element that can be pressed, turned, or slid is a *button*.
The *Button Definition* is a list of parameters that describe what the button will do when it is manipulated and how it will be represented on the deck if the deck can some how represent the state of that button. Here is an example of button definition:

```
buttons:
  - index: 2
    name: MASTER WARNING
    type: push
    command: sim/annunciator/clear_master_warning
    annunciator-animate:
      text: "MASTER\nWARN"
      text-color: firebrick
      text-font: DIN Condensed Black.otf
      text-size: 72
      dataref: AirbusFBW/MasterWarn
      animation-speed: 2.0
```

The following section describe common button definition attributes.
The button *Activation* page describes attributes specific to what a button will trigger or do when used.
The button *Representation* page describes attributes specific to the *rendering* of the button on the deck.

# Button (Common) Attributes

## Button Index
Each «Button» on a deck is designated by its [[Button Index|Index]].
On a given Page, the Index of a button must be unique on that Page since it addresses a very precise key, knob or slider on the deck.
Depending on the model of deck, buttons have  [[Button Index|coded named index]].
On a simple deck with a number of similar keys, the index of a button is its ordering number: 0, 1, 2... until the number of keys is reached. On a more complex deck, with button and knobs, knobs may be indexed with name like knob0, knob1, knob2... until the number of knobs is reached.
The Index of a button is a mandatory parameter of the Button definition. There is no default value.

## Button Name
Optional. A button can be named.
The name of a button on a page must be unique. If more than one button have the same name, an error is reported and the definition of the button is ignored.
If no name is provided, a unique, long, technical name is created from deck name, page name, and index.

## Button Type
Mandatory. The type of a button defines what the button will do and how it will be used.

The [[Button Activation|button activation]] describe button-type specific attributes. In other words, depending on the value of the type attribute, other button defining attributes will be expected.

## Button Label
The label of a button is a short reminder of what the button does. The text of the label is laid over the button image if any. The labelling of a button uses the following attributes:

```
    label: SHORT TITLE
    label-color: white
    label-size: 24
    label-font: Seven Segment.ttf
    label-position: ct
```

Note: The Button *Label* should not be confused with Button Text. The Label exist for all buttons, and is displayed according to its attributes if the underlying button is capable. The text of the label is defined as a button attribute and is static (cannot be changed dynamically).
The Button *Text* is a text that is part of the Button representation.

#### Label Color
See [[Cockpitdeck Resources#Colors|Colors]].

#### Label Font
See [[Cockpitdeck Resources#Fonts|Fonts]].

#### Label Size
In pixels. Internally, Cockpitdecks uses 256 × 256 pixel images.

#### Label Position
The position of the label is a 2 letter code:
1. l, c, or r for left, center, or right-justified on the image (horizontal alignment),
2. t, m, or b, for top, middle, or bottom of the image (vertical alignment).

# Button Value
A Button has a value that is maintained and used mainly for representation.
Please head [[Button Value|here]] for details about a button's value computation.

## Button Initial Value
A Button can force its first, initial value to set its startup or original state.

```
	initial-value: 2
```

This value is assigned as the button's current value on startup.
In case of a Button with multiple values, each value has a independant `initial-value` attribute in its own attribute section.

# Button State
Each button maintain its internal state: How many times it is pressed, released, turned clockwise or counter-clockwise, what is it current value, its previous, or last value. State information can be used by Button designer to control the button behavior and its representation.

1. activation_count (number of time button was pressed.)
2. current_value
3. previous_value

*(Please refer to button activations, each activation type returns its own set of state values.)*

# Button Activation

The `type` attribute of a button determine [[Button Activation|how the button will behave]], what it will do when pressed, turned or slid.

# Button Representation

The representation of a Button determine [[Button Representation|what and how the button will display]] on the deck device. This depends on the capabilities of the button on the deck: LED, image, coloured led button, sound...
The representation of a button is determined by *the presence of a special attribute in the definition of the button*.
For example, if a button definition contains an attribute named `annunciator`, the button representation will be an [[Annunciator]]. A button can only define one representation in its definition. Otherwise, a warning is reported and the button is ignored.

# Button Instantiation

When a button is created, internal meta data are set first. Second, the Activation is installed and initialised. Third, the Representation is installed and initialized, as it may already use some activation information for rendering. Finally, the button is initialised. It will be rendered when the page that contains it is activated on a deck.

# Button Validity

Each button has a validity function that ensures that all necessary attributes are provided in its definition. If the activation of the button is not valid, its activation function will never be triggered, because of missing or misconfigurated parameters. If its representation is not valid, it will not be rendered on the deck.
If a button is not valid, a small red triangle appears in the lower right corner of the key icon if the button is capable of representing it. A small blue triangle appears in the lower right corner of the key icon if Cockpitdecks suspect the button is a placeholder.

# Button Description and Inpection

Each button has an `describe()` method that prints in plain English what the button does and what it renders on the deck.

Each button has an `inspect(what: str)` method that exposes internal values and state. The inspect method takes one parameter `what`  that determines what is displayed when invoked.

These methods can be invoked from the [[Button Activations for Developers#ButtonInspect|Inspect]] button activation.

