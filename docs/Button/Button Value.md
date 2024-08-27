A button builds its representation from its *value*. The *value* of the button is computed from one or more *dataref* values returned by X-Plane and/or from some *internal state variable* values.

A Button can have 0, 1, or more than one value in the special case of [[Annunciator|annunciators]] or LargeButtons  (A LargeButton  can have two or more buttons represented inside.). Each annunciator part or each button inside a LargeButton has either 0, or 1 value.

Each value of a button is either None (no value) or a numeric value (which is most of the time a floating point number). If a button has several values, its value is either a list of values or a dictionary of all individual values, each individual value being None or a number.

Activations and Representations of the button knows how to manage the different values contained in the annunciator or LargeButtons.

# X-Plane Datarefs

A *dataref* is the name of a value used by the X-Plane simulator.

The value can be a string, integer, or float value, either a single value or an array of (same type of) values. A dataref has a name to access it. Names are organized in a folder-like structure (namespace using `/` separator). Some datarefs are read-only, some other can be written and modified.

#### Examples

| Dataref name                       |Value| Description                                                                     |
| ---                                |---| ---                                                                             |
| sim/cockpit/misc/barometer_setting | Float |Value of the atmospheric pressure at the aircraft location in inches of mercury |

There are thousands of datarefs in a running instance of X-Plane. Datarefs drive almost everything in the simulator.

A dataref is always monitored. Its value is fetched from the simulator at regular interval (typically every second). When a dataref's value has changed, all buttons that depend on that dataref are notified to update their appearance.

To explore datarefs, there is a handy X-Plane plugin called [DataRefTool](https://datareftool.com). There are also a few web pages that collect, report, and present them so that they can be searched.

For simplicity, Cockpitdecks assumes all individual dataref values are floating point numbers or strings (in which case it actually is a list of floating point values representing the ASCII code number of each character in the string.)

The reason for this is that as today, X-Plane UDP only returns floating point numeric values for requested datarefs.

## Dataref Rounding

Dataref values can change insignificantly very rapidly. To prevent dataref update and its consequences (update of the button value and its representation) with the value change insignificantly, dataref values can immediately be rounded before they are communicated to Cockpitdecks.

Required rounding is expressed in the `config.yaml` file.

The `dataref-roundings` attribute is a list of (dataref-name, significant decimal digit after comma):

```yaml hl_lines="1"
dataref-roundings:
    sim/cockpit/autopilot/heading_mag: 2
    sim/flightmodel/position/latitude: 8
    sim/flightmodel/position/longitude: 8
    sim/cockpit/misc/barometer_setting: 2
```

There is a global, application-level, `dataref-roundings` located in the main Cockpitdecks resources folder. The file should never be changed or touched.

It is possible for the aircraft configuration developer to specify particular rounding needs in the main `config.yaml` file in the aircraft `deckconfig` folder in the same way. Aircraft roundings will take precedence on global roundings.

Dataref roundings only applies to datarefs fetched from X-Plane.

## Dataref Fetch Frequency

Similarly to dataref roundings, it is possible to specify, on a dataref basis, the frequency at which Cockpitdecks will ask X-Plane to send values in UDP packets.

The default values is between 1 and 4 times per seconds, 1 to 4 Hz.

```yaml hl_lines="1"
dataref-fetch-frequencies:
    sim/cockpit/autopilot/heading: 10
```

## X-Plane / Cockpitdecks String Dataref

Please refer to [[String Datarefs|this Section]] for The special handling of string datarefs.

# Cockpitdecks «Internal» Datarefs

Cockpitdecks manages its own set of *internal datarefs*.

All datarefs that have a path or name that starts with a special key word are *NOT* forwarded to X-Plane but rather managed internally inside Cockpitdecks. Otherwise, they are not different from X-Plane datarefs. They can be set and used like any other datarefs.

When a button produces an internal dataref, it's definition mention it clearly so that it can be used by other buttons.

The current default prefix for internal datarefs is `data:`.

```yaml
   set-dataref: data:my-local-variable

# in the same or another button, it can be used like so:

   formula: ${data:my-local-variable}
```

In the above example, the prefix `data:` denotes internal datarefs. The name of the internal dataref is `data:my-local-variable`.

Internal datarefs can be used as inter-button communication, to set a value in one button, and use or read it in another one.

# Button «Internal State» Attributes

When a button cannot fetch its representation from X-Plane, it is possible to use some Cockpitdecks internal variables made available through the button *state*. Each button maintain its state, a few internal variables’that can be accessed in formula.

Some state variables are generic, and available for almost every buttons, like for instance the number of time a button was activated. Other state variables are activation specific and listed in the [[Button Activation]] page under the activation being used, like for example, the number of times a encoder was turned clockwise.

Numeric internal values are accessible as `${state:variable-name}` in formula.

```yaml
	formula: {state:button_pressed_count} 2 mod
```

## Activation Attributes and Activation Value

Most state attributes of a button come from the activation. Each activation has a specific set of state attributes. Among these state attributes, there is a more particular attribute, called the *activation value*, which is the most sensible value produced by the activation.

## Class Instance Attributes

For Cockpitdecks developers, all attributes used in the button, its activation, or its representation class instances are also available as state variables. In this case, the value of the attribute is returned with no type checking.

```python hl_lines="3"
class SpecialActivation:
    ACTIVATION_NAME = "my-activation"
    def __init__(self):
        self.my_value = 8
```

When used:

```
my-activation:
    formula: ${state:my_value} 2 /
```

equals 4.

# Button Value

From the above

- X-Plane datarefs
- Cockpitdecks internal datarefs
- Button internal state attributes, in particular the activation value

which may be called *variables*, a button provides a final value. This value is supplied to the representation that will provide the button feedback on the deck.

The following sections details how the value gets computed from the above variables. The possibilities are:

- Value from a formula (that combines several datarefs and button internal state attributes),
- Value from a single dataref,
- Value returned by the activation,
- A list of values for a representation that requires it,
- or finally, a list of all the above variables in a dictionary of values.

## Single Dataref Value

The value of the button is determined by a single dataref value.

```
dataref: sim/weather/region/atmospheric_pressure
```

## Combining Multiple Variables with Formula

A Representation is driven by a *single final value*. However, it is possible to compute that final value form a list of dataref values, internal button attributes, and mathematical operations. This is done through a `formula` attribute. The formula is written in [Reverse Polish Notation](https://en.wikipedia.org/wiki/Reverse_Polish_notation), a method to write and execute operations on values. Since a formula allows for value transformation, a formula should always produce a value that is directly usable by a representation. The value of a button can be computed from data coming either from X-Plane (through dataref values) and/or from the button's internal state values.

Examples of `formula`:

```yaml
# Simple math (in reverse polish notation):
formula: ${AirbusFBW/OHPLightsATA34[8]} 2 * floor

# Constant 1; always 1; always True or On
formula: 1

# Insert 0 = true, 1 = false
formula: ${sim/cockpit2/switches/avionics_power_on} 1 - abs

# Boolean operation not
formula: ${sim/cockpit2/switches/avionics_power_on} not

# Boolean operation
formula: ${AirbusFBW/OHPLightsATA34[8]} 4 eq

# Formula used for display of a value
formula: ${sim/cockpit/misc/barometer_setting} 33.8639 *
text: ${formula}
text-format: "{: 4.0f}"

# The following two lines are equivalent; they both return the same value
formula: ${sim/cockpit/autopilot/vertical_velocity}	
dataref: sim/cockpit/autopilot/vertical_velocity
```

Only one formula attribute can be used for a button or a annunciator part or a LargeButton button.

### Expression

The *formula* for computation is expressed in *Reverse Polish Notation*. The result of the formula is a *numeric value* (float value that can be rounded to an integer if necessary.) It is intimidating at first to write RPN formula, but once a user get use to it, it actually is equaly easy to write RPN formula and formula with parenthesis. In a nutshell, rather than writing:

```
(8 / 2) + (4 × 5)
```

in RPN, we write:

```
8 2 / 4 5 × +
```

We place the value we act upon first, then the operation we perform on those values.

### Operators

The following operator have been added:

- `%` or `mod`: Pushes reminder of division of last two elements, modulo.
- `floor`: Round element to smaller integer value.
- `ceil`: Round element to larger interger value.
- `round`: Last element rounded to closest integer value.
- `roundn`: Element rounded to last element of stack (forced to interger):
  `1.2345 2 roundn => 1.23`
- `abs`: Absolute value of last element
- `chs`: Change sign of last element
- `eq`: Test for equality of last two elements. Pushes 1 for True, 0 for False on the stack.
- `not`: Boolean not operator, insert 1 if it was 0 or 0 otherwise.

### Variable Substitution

In formula:

- `${dataref-path}` is replaced by the scalar value (converted to float) of the dataref pointed by `dataref-path`. Example: `${sim/aircraft/fuel/tankleft}`.
- `${state:name}` is replaced by the scalar value of the (current) button' state variable named `name`. Names of available state variables depend on the activation; each activation lists internal state variables made available through the button' state. Example: `${state:activation_count}`.
- ~~`${button:cockpit-name:deck-name:page-name:button-name:button-variable-name}`:  Substitute de given button name by its value. Example:  `${button:Airbus A321:sd-xl:efis:apu:status:activation_count}` . If no button variable name is given, the current value of the button is returned.~~

In all case, if the value is not found, it is replaced by None, which translate into 0 (zero) in formula (to prevent the formula from failing to compute). If the value is not found, a warning message is reported.

The following formula determine the final status On(=1) or Off(=0) from the number of times a button was pressed:

```
formula: ${state:activation_count} 2 %
```

## Activation Value

If a button does not reference a dataref, and has no formula, the activation value can be used if it is available.
## Multiple Button Values

In case a button has multiple values, each value comes from a part of the button. Each part of the button is independent of other parts of the same button. Each part maintains its single value.

All part values are aggregated into either a *dictionary* or an *array* of values that is made available at the button level.

For example, Annunciator, or LargeButton representation, have more than one individual values that are fetched and maintained to provide a single table (or array) of individual values.

Another example is a button that has more than one dataref and no formula. In this case, the returned value is a dictionary of all dataref values of that button.

## Button Initial Value

A Button can force its first, initial value to set its startup or original state.

```
	initial-value: 2
```

This value is assigned as the button's current value on startup.

In case of a Button with multiple values, each value has a separate `initial-value` attribute.

## Button with No Value

Some button may not maintain any state or use any value. Example of such button are simple push button with no feedback mean.

## Summary

The following depicts how a button's value is computed:

![[compute value.svg|500]]

## Notes about Value (Re-)Computation

The value of a button is updated at the following moment:

1. When the button is initialized, a first value is computed if not supplied.
2. When a dataref on which the button depends has changed.
3. After an activation.

An activation only modifies its internal state and does not "forward" its modification to the button. It is the button's responsibility to fetch the value it needs in the activation, through, for example, a state variable:

```
formula: ${state:counter-clockwise-movement-count}
```
