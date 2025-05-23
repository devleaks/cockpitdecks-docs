A Variable is a named value used by cockpitdecks.

A Variable has *listeners* attached to it. Listeners are other parties interested in being notified when the variable has changed. Listeners are managed by Cockpitdecks.

# Variable Types

## Cockpitdecks Internal Variable

Cockpitdecks internal variables are variables that are mainly used inside Cockpitdecks code for internal introspection. However, it is possible to declare and use an internal variable in the configuration files.

The name of an internal variable start with `data:`.

The attribute to declare variables are:

- `dataref` for historical reasons,
- `datarefs` or `multi-datarefs`,
- `string-datarefs` for string-type variables (they must be declared BEFORE other type variables.)

It can be declared as a string variable:

```
	dataref: data:encoder0_value
```

> [!NOTE] Internal Variable name
> The name of an internal variable starts with `data:`. It is a convention, and it is how internal variables are detected and treated separately.

## Simulator Variable

Simulator variables are variables that are requested from the simulator.

A special mechanism request the variable value from the simulator, and when the value changes, Cockpitdecks notifies variable listeners with the new value.

```
	dataref: AirbusFBW/ExtPowOHPArray[0]
```

## Button (Internal) State

Button internal states are special variables exposed by buttons. Their name starts with `state:`.

```
    dataref: state:activation_count
```

The name of the states available depend on the button, its activation and representation types.

## Formula

A Formula is a special variable made of other variables. The value of the formula results from the evaluation of a expression (hence the name formula) that combines the variables.

Expression are written in Reverse Polish Notation.

Formula are sometimes as simple as expression like:

```
    formula: ${sim/cockpit2/gauges/actuators/barometer_setting_in_hg_pilot} 33.86389 * round
```

to compute a new value based on another one.

However, expression can be as complex as needed.

```
    formula: ${AirbusFBW/Pack1SwitchIllum} ${AirbusFBW/Pack1FCVInd} - abs
```

When the value of one of the constituing variable changes, the resulting value of the whole formula is changed.

# Variable Value

For practical reasons, variable values are mainly limited to two data types:

- **String** for character string variables, like for example the `sim/aircraft/view/acf_ICAO` X-Plane dataref that has the value of the ICAO code of the aircraft currently being used.
- **Float** for other, numeric variables. Float is the default data type.

Variable a created when they are requested and accessed by their name. The first time a variable is requested it is created with the proper type (string or float). The data type of the variable cannot be changed.

It is therefore very important to first declare the variable with its proper type before using it. This is particularly true with string variable, because if not explicitly declared as string variable before it is used, it will have the default float data type.

# Variable Datase

When a variable is first requested, it is added to a database of all variables used by Cockpitdecks. If the same variable is requested at other places or time, the same instance of the variable is used. There is one and only one variable instance for a variable name.

The variable database is located in the Cockpit.
