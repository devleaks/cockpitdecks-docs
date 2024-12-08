A *button* is the general term for a key, knob, rotary encoder, slider cursor, or even touch surface on a deck. On a given deck, each element that can be pressed, turned, swiped, or slid is a *button*.

> [!NOTE] May be the term *Interactor* would have been better, more abstract, more generic, but he first deck we used only had keys to be pressed, so we sticked to the term *Button*, and extended the interactions a simple Button allow.

For a given deck, all buttons that are available and/or displayed at a moment in time are on the same *Page*, the collection of buttons currently usable on that deck. Hence, in the definition of a page, there is a mandatory `buttons` attributes that lists all buttons on that page.

In that list, each button is defined by a list of attributes that will determine what it does and how it appears on the deck. The list of attributes that define a button is called a *Button Definition*.

# Button Definition

The *Button Definition* is a list of attributes that describe what the button will do when it is manipulated and how it will be represented on the deck if the deck can some how represent the state of that button.

Button definition can be very simple and straightforward, but definitions can also be complex and refined.

Here is an example of a simple definition of a button to toggle the map display in X-Plane.

```
index: 0
type: push
command: sim/map/toggle_map_display
icon: map
```

The definition of a button can be organised into 4 distinct parts:

1. What the button is (and where it is on the deck)
2. What it does (when the button is pressed on the deck)
3. What it displays (on the deck)
4. Where does it get the value it displays from.

Here is a complex definition of a button which exemplifies all above parts (in different colors):

![[button-anatomy.png]]

Resulting button:

![[button.png|200]]

The following Sections describe the four button definition parts in detail.

# Common Button Attributes

(In the *Anatomy of a Button* above, this refer to the blue part of the definition.)

| Attribute        | Definition                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `index`          | **Mandatory**. There is no default value.<br><br>Each «Button» on a deck is designated by its [[Button Index]]. So the index designate a very precise button on the deck.<br>On a simple deck with a number of similar keys, the index of a button is its ordering number: 0, 1, 2... until the number of keys is reached. On a more complex deck, with button and knobs, knobs may be indexed with name like knob0, knob1, knob2...                                                                                                                                    |
| `name`           | Optional. A button can be named to ease its identification.<br><br>The name of a button on a page must be unique. If more than one button have the same name, an error is reported and the definition of the button is ignored.<br><br>If no name is provided, a unique, long, technical name is created from deck name, page name, and index.                                                                                                                                                                                                                          |
| `type`           | **Mandatory**.<br><br>The type of a button defines what the button will do and how it will be used.<br><br>The [[Button Activation]] describe button-type specific attributes. In other words, depending on the value of the type attribute, other button defining attributes will be expected.<br><br>For example, if the value of a button type is `page` to change a page on a deck, it is expected to find the attribute named `page` which contains the name of the page to switch to when pressed.                                                                |
| `label`          | The label of a button is a short reminder of what the button does. The text of the label is laid over the button image if any. The labelling of a button uses the following attributes:<br><br>Note: The Button *Label* should not be confused with Button Text. The Label exist for all buttons, and is displayed according to its attributes if the underlying button is capable. The text of the label is defined as a button attribute and is static (cannot be changed dynamically).<br><br>The Button *Text* is a text that is part of the Button representation. |
| `label-color`    | See [[Resources#Colors]].                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| `label-font`     | See [[Resources#Fonts]].                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| `label-size`     | In pixels. Internally, Cockpitdecks uses 256 × 256 pixel images.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| `label-position` | The position of the label is a 2 letter code:<br><br>1. l, c, or r for left, center, or right-justified on the image (horizontal alignment),<br>2. t, m, or b, for top, middle, or bottom of the image (vertical alignment).                                                                                                                                                                                                                                                                                                                                            |
| `options`        | Regularly, buttons have additional parameters.<br><br>The button options parameter is a string of comma separated options. An option is either a simple string or word, or a name=value string.<br><br>Options are, by nature, not indispensable to the button’s activation or rendering but rather add to it to alter behaviour or appearance.                                                                                                                                                                                                                         |
| `view`           | The view attribute is an additional optional instruction that is executed *after* the activation. The idea behind it is to first perform the action and then change the simulator view to focus on an area of interest and the activation has completed.                                                                                                                                                                                                                                                                                                                |

# Button Value

(In the *Anatomy of a Button* above, this refer to the yellow part of the definition.)

A Button has a value that is maintained and used mainly for its representation.

The value of a button can come from two sources:

- The simulator (i.e. some variable or parameter used and controlled by the simulator)
- The internal state of the button (read below)

The following attributes are used to determine a button’s value:

`dataref` : A single dataref value.

`formula`: An expression that contains variables, including datarefs, and mathematical operations to combine and compute a single final value.

`multi-datarefs`: A list of two or more datarefs. The values of all datarefs in the list will be returned as the value of the button. In this case, the value will be a *composite* value. The representation that uses such a specific value will know how to use each part of the composite value.

## Value from the Simulator

When the value of a button is computed from one or more values coming from the simulator, Cockpitdecks will request the values from the simulator, and each time one of these values changes, Cockpitdecks will notify the button of the changes so that it can adjust its representation.

## Button State

Finally, in addition to the above attributes that can be used to specify the value of the button, a button has a set of internal attributes that can also be used to determine its value.

Each button maintain an internal state: How many times it is pressed, released, turned clockwise or counter-clockwise, what is it current value, its previous, or last value, when it was last used or refreshed, etc.. State information can be accessed by Button designer to control the button behavior and its representation, or its value.

Examples of internal state attributes are:

1. `activation_count` (number of time button was «used»)
2. `current_value`
3. `previous_value`

Internal state attributes varies depending on the [[Button Activation|button activation]]. Each activation type returns its own set of particular state values.

All these attributes can be used either individually or combined in a formula to determine the value of a button.

Please head [[Button Value|here]] for details about the computation of the value of a button.

## Button Initial Value

A Button can force its first, initial value to set its startup or original state.

```yaml
	initial-value: 2
```

This value is assigned as the button's current value on startup.

In case of a Button with multiple values, each value has a separate `initial-value` attribute in its own attribute list.

# Button Activation

(In the *Anatomy of a Button* above, this refer to the green part of the definition.)

The `type` attribute of a button determine [[Button Activation|how the button will behave]], what it will do when pressed, turned or slid.

## Set-Dataref

A button definition can have a `set-dataref` attribute that points at a Dataref name.

If present, Cockpitdecks will set the value of that dataref to the value of the button each time the value of the button changes.

Here is example of use. If a button has a activation type of `updown` with let us say 3 stops, the value of the button can be 0, 1, or 2. Each time the user presses the button the value of the button cycles between those three values.

```
type: updown
stops: 3
set-dataref: toliss/NDmodeFO
```

If there is a `set-dataref` attribute, the current value of the dataref (`toliss/NDmodeFO`) in the simulator will be set to the value of the button: 0, 1, or 2.

For some activations, the `set-dataref` attribute is a mandatory attribute.

For some other activations that expects a command to be performed upon activation, the `set-dataref` attribute (or instruction) can be an *alternative* to the command. In other words, the commands that gets executed is very precisely the `set-dataref` instruction.

# Button Representation

(In the *Anatomy of a Button* above, this refer to the pink part of the definition.)

The representation of a Button determine [[Button Representation|what and how the button will display]] on the deck device. This depends on the capabilities of the button on the deck: LED, image, coloured led button, sound...

The representation of a button is determined by *the presence of a special attribute in the definition of the button*. That attribute will determine how the button will be represented.

For example, if a button definition contains an attribute named `annunciator`, the button representation will be an [[Annunciator]]. A button can only define one representation in its definition. Otherwise, a warning is reported and the button is ignored.

Attributes related to the representation of the button are indented under the attributes that names the representation. This is done on purpose to clearly separate attributes dedicated to what the button does (activation), and how it provides feedback to the user (representation).

A Page of 32 buttons 4 rows of 8 buttons) can quickly become quite large and difficult to read. Fortunately, recall that a Page can include other Pages to structure the creation of a layout. For example, it might be advisable to create a « main » page with global settings, and include four sub-pages, one for each row on the deck.
