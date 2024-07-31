 A Button can have 0, 1, or more than one value in the special case of [[Annunciator|annunciators]] or LargeButtons  (A LargeButton can have two or more buttons represented inside.). Each annunciator part or each button inside a LargeButton has either 0, or 1 value.

Each value of a button is either None (no value) or a numeric value (which is most of the time a floating point number). If a button has several values, its value is either a list or a dictionary of all individual values, each individual value being None or a number.

Activations and Representations of the button knows how to manage the different values contained in the annunciator or side button.

A button builds its representation from its *value*. The *value* of the button is computed from one or more dataref values returned by X-Plane and/or from some internal state variable values.

# Value Sources

A Button can get its value from the following sources:

1. A single X-Plane *Dataref*
2. A *formula* that combines both dataref values and/or button internal state values

There should never be both a single dataref and a formula. It is either one or the other. In case both are defined, the formula takes precedence.

If no `dataref`  for `formula` attribute determine the value of a button, it will return all values of its internal state in a dictionary of values.

## X-Plane Dataref

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

## X-Plane / Cockpitdecks String Dataref

Please refer to [[String Datarefs|this Section]] for The special handling of string datarefs.

## Cockpitdecks Internal Dataref

Cockpitdecks manages its own set of *internal datarefs*.

All datarefs that starts with a special key word are *NOT* forwarded to X-Plane but rather managed internally inside Cockpitdecks. Otherwise, they are not different from X-Plane datarefs. They can be set and used like any other datarefs.

When a button produces an internal dataref, it's definition mention it clearly so that it can be used by other buttons.

The current default prefix for internal datarefs is `data:`.

```yaml
   set-dataref: data:my-local-variable

# in the same or another button, it can be used like so:

   formula: ${data:my-local-variable}
```

In the above example, the prefix `data:` denotes internal datarefs. The name of the internal dataref is `data:my-local-variable`.

Internal datarefs can be used as inter-button communication, to set a value in one button, and use or read it in another one.

## Internal Button (Activation) Value

When a button cannot fetch its representation from X-Plane, it is possible to use some Cockpitdecks internal variables made available through the button *state*. Each button maintain its state, a few internal variables’that can be accessed in formula.

Some state variables are generic, and available for almost every buttons, like for instance the number of time a button was activated. Other state variables are activation specific and listed in the [[Button Activation]] page under the activation being used, like for example, the number of times a encoder was turned clockwise.

Numeric internal values are accessible as `${state:variable-name}` in formula.

```yaml
	formula: {state:button_pressed_count} 2 mod
```

### Class Instance Attributes

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

## Summary

The following depicts how a button's value is computed:

![[compute value.svg|500]]

# Value Normalisation

The raw value acquired from a source may not be usable by a button representation. Before a raw value can be used by a representation, it needs *normalisation*. The normlaisation of a value is the process of transforming the (raw) value from the datarefs or formula in values usable for representation.

### Rounding

Some dataref values are changing very rapidly over time, sometimes in insignificant changes. To prevent updating a button's state each time a value changes, a Dataref value can be rounded before it enters Cockpitdecks processing.

```
sim/weather/aircraft/qnh_pas 0 round
```

Value 101308.35278 will be rounded to 101308.

### Examples of Normalisation Need

- a LED can can be turned On or Off need to receive a boolean value, On or Off as an indicator to light the LED or not, while the formula combining datarefs might return a float value.
- a switch that can have 5 different positions must receive a value between 1 and 5 to determine which position to represent. Not 0, not 6, which are invalid values for its representation.

# Button with No Value

Some button may not maintain any state or use any value. Example of such button are simple push button with no feedback mean.

# Single Dataref Value

The value of the button is determined by a single dataref value.

## Binary Value

In its simple form, the value of a button is deduced from the value of a single dataref. If the value of the dataref is zero, the representation is OFF, otherwise, it is ON.

## Continuous Range Value

The status of a button is deduced from the value of either a single dataref, or a formula expression.

Ranges are used as buckets to determine in which range/bucket the value falls. Then, the number index of the range, starting with 0, is used to determine the button’s value.

# Combining Multiple Values: Formula

A Representation is driven by a *single final value*. However, it is possible to compute that final value form a list of dataref values and mathematical operations. This is done through a `formula` attribute. The formula is written in [Reverse Polish Notation](https://en.wikipedia.org/wiki/Reverse_Polish_notation), a method to write and execute operations on values. Since a formula allows for value transformation, a formula should always produce a value that is directly usable by a representation. The value of a button can be computed from data coming either from X-Plane (through dataref values) and/or from the button's internal state values.

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

## Expression

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

## Variable Substitution

In formula:

- `${dataref-path}` is replaced by the scalar value (converted to float) of the dataref pointed by `dataref-path`. Example: `${sim/aircraft/fuel/tankleft}`.
- `${state:name}` is replaced by the scalar value of the (current) button' state variable named `name`. Names of available state variables depend on the activation; each activation lists internal state variables made available through the button' state. Example: `${state:activation_count}`.
- ~~`${button:cockpit-name:deck-name:page-name:button-name:button-variable-name}`:  Substitute de given button name by its value. Example:  `${button:Airbus A321:sd-xl:efis:apu:status:activation_count}` . If no button variable name is given, the current value of the button is returned.~~

In all case, if the value is not found, it is replaced by None, which translate into 0 (zero) in formula (to prevent the formula from failing to compute). If the value is not found, a warning message is reported.

The following formula determine the final status On(=1) or Off(=0) from the number of times a button was pressed:

```
formula: ${state:activation_count} 2 %
```

# Multiple Button Values

In case a button has multiple values, each value comes from a part of the button. Each part of the button is independent of other parts of the same button. Each part maintains its single value.

All part values are aggregated into either a *dictionary* or an *array* of values that is made available at the button level.

For example, Annunciator, or LargeButton representation, have more than one individual values that are fetched and maintained to provide a single table (or array) of individual values.

Another example is a button that has more than one dataref and no formula. In this case, the returned value is a dictionary of all dataref values of that button.

# Button Initial Value

A Button can force its first, initial value to set its startup or original state.

```
	initial-value: 2
```

This value is assigned as the button's current value on startup.

In case of a Button with multiple values, each value has a separate `initial-value` attribute.
