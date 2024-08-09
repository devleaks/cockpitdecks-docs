In the definition of the button, the `type` attribute determine what the button will do.

For a given button on a deck, the action that the button will be able to carry over is limited to a set of valid types. A push button is not capable of rotations of an encoder.

When interaction occurs on a button on a deck, Cockpitdecks creates a typed Event, and passes it for activation.

Each activation is designed to handle one or more [[Events]] types. For example a EncoderPush activation is capable of handling both PressEvent and EncoderEvent. What is does with those event is left to the activation.

This file list all activations that are currently available in Cockpitdecks. For each activation, we present its name or keyword by which it must be referred to, the *attributes* it expects to work properly, the type of *events* it expects, and internal *state values* it produces and maintains.

- [[Button Activation#No Activation]]
- [[Button Activation#Page]]
- [[Button Activation#Push]]
- [[Button Activation#Longpress]]
- [[Button Activation#OnOff]]
- [[Button Activation#UpDown]]
- [[Button Activation#Encoder]]
- [[Button Activation#EncoderPush]]
- [[Button Activation#EncoderOnOff]]
- [[Button Activation#EncoderLongPush]]
- [[Button Activation#EncoderValue]]
- [[Button Activation#Slider]]
- [[Button Activation#Swipe]]

# No Activation

`type: none`

Button with no activation are button used for [[Button Value|display purpose only]].

## Events

Any event can be handed over to the No Activation, since it will not be used.

## State Values

| State Variable     | Value                                                                                                         |
| ------------------ | ------------------------------------------------------------------------------------------------------------- |
| `activation_count` | Number of time button was activated                                                                           |
| `last_activated`   | Time stamp of last activation                                                                                 |
| `initial_value`    | Initial value in configuration (if any)                                                                       |
| `current_value`    | Button current value                                                                                          |
| `previous_value`   | Button previous value before change                                                                           |
| `guarded`          | Whether button has a guard on top of it                                                                       |
| `managed`          | Whether button has *managed* mode (specific to some cockpits, which means there are alternate display values) |

# Page

`type: page`

When the button is pressed, a deck will load a page of buttons.

## Attributes

| Attribute     | Definition                                                                                 |
| ------------- | --------------------------------------------------------------------------------------- |
| `page`        | Mandatory. Name of the page to load. The page must be in the Layout of the target deck. |
| `remote_deck` | Optional. If present, will load the page on the target deck.                            |

## Events

`PushEvent` and `PressEvent` can trigger the Page activation.

## State Values

| Attribute | Definition                          |
| --------- | -------------------------------- |
| `page`    | page that is currently displayed |

# Theme

`type: theme`

When the button is pressed, the main theme will be changed.

## Attributes

| Attribute | Definition                |
| --------- | ------------------------- |
| `theme`   | Mandatory. Name of theme. |

## Events

`PushEvent` and `PressEvent` can trigger the Page activation.

# Push

`type: push`

Push button.

## Attributes

| Attribute | Definition                                                                      |
| --------- | ---------------------------------------------------------------------------- |
| `command` | Mandatory. X-Plane command that is executed each time the button is pressed. |

> [!NOTE]
> Command is a mandatory parameter but if no command is necessary a command placeholder value can be used. Command placeholder value are any of the following string:
> `none, noop, no-operation, no-command, do-nothing`
> They all are ignored and do not trigger any activity in X-Plane.

## Events

PushEvent. Please note that PushEvent consists of 2 distinct events, a pressed event (PushEvent with pressed = True), and a release event (PushEvent with pressed = False), when the button is pressed or released respectively.

## Options

| Option        | Definition                                                                                                                                                                                                                                                                                              |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `auto-repeat` | Auto repeat command at specified pace while the button remains pressed.<br>`auto-repeat` option accepts a couple of optional values:<br><br>- `delay`: Time (in second) after which the auto-repeat starts, default to 1 second.<br>- `speed`: Time (in second) between 2 executions of the command. |

```
	options: auto-repeat=3/0.5,dot
```

Auto-repeat will start 3 second after the button was pressed, the command will auto-repeat every 0.5 seconds, twice per second. (`dot` option also set for other purpose.)

# Longpress

`type: longpress`

Push button that will carry the command as long as the button will remain pressed.

Long press command should not be confused with auto-repeat commands. A Longpress command in one command that is executed once as long as the button remain pressed. An auto-repeat command is the same command that is executed several times at regular interval (typically once every 0.2 seconds, 5 times per second) as long as the button is pressed.

> [!NOTE]
> The Longpress command requires installation of a XPPYthon3 plugin in X-Plane to circumvent a few X-Plane UDP limitations.

## Events

PushEvent

Only PushEvent can be used to trigger Longpress Activation since both press and release events are necessary to estimate the timing between both events.

## Attributes

| Attribute | Definition                                                                      |
| --------- | ---------------------------------------------------------------------------- |
| `command` | Mandatory. X-Plane command that is executed each time the button is pressed. |

X-Plane will issue a `beginCommand` when the button is pressed and a `endCommand` when released.

> [!NOTE]
> Please note that the use of longpress command needs the addition of a little plugin to circumvent X-Plane UDP limitations when used through UDP.

# OnOff

`type: onoff`

Push button.

## Events

PushEvent, PressEvent, LongPressEvent

## Attributes

| Attribute     | Definition                                                                                                                                                                |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `commands`    | Optional pair of X-Plane commands that are executed alternatively. Two commands must be supplied, but the same command can be provided twice.                          |
| `set-dataref` | Optional dataref to set On(=1) or Off (=0).<br><br>Either attribute can be set or both. In the latter case, the command is first executed and then the dataref is set. |

## State Values

| Attribute | Definition              |
| --------- | ----------------------- |
| `on`      | current state On or Off |

# UpDown

`type: updown`

Cycle Up and Down button.

## Attributes

| Attribute       | Definition                                                                                                                                                                                                                                                                     |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `commands`      | X-Plane commands that are executed when pushes increase value, and when pushes descrease value.                                                                                                                                                                             |
| `stops=3`       | Number of stop values. For example: Stops=3 will give 0-1-2-1-0 cycles, with 3 stops 0, 1, and 2.                                                                                                                                                                           |
| `initial-value` | If an initial value is supplied, it's sign indicated how the value will evolve.<br><br>For example, if the initial value is 1, the next value will be 2 (go up). If the initial value is -1, the initial value will be set to 1, but the next value will be zero (go down). |
| `set-dataref`   | Optional dataref to set the value of the current stop.<br><br>Very much like On/Off activation either `commands` or `set-dataref` can be supplied or both.                                                                                                                  |

## State Values

| Attribute | Definition                                           |
| --------- | ---------------------------------------------------- |
| `stops`   | Number of stops                                      |
| `go_up`   | True will increase at next push; False will decrease |
| `stop`    | Current value                                        |

# ShortOrLongpress

Short or long press has two commands, one that is executed when the button is pressed for less than `long-time` seconds, and one when it is pressed more than `long-time` seconds.

`type`: short-or-longpress

## Attributes

| Attribute   | Definition                                                                 |
| ----------- | -------------------------------------------------------------------------- |
| `commands`  | Two commands, the first one is called on short press.                      |
| `long-time` | Time to press the button to activate second command. Default to 2 seconds. |

# Encoder

`Type: encoder`

An Encoder is a rotating knob or dial with *steps*. Steps are often materialised by a little sound or a slight resistance in the rotation.

## Attributes

| Attribute  | Definition                                                                                                                                        |
| ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| `commands` | An Encoder has two commands, one that is executed for each step while turning clockwise, and one for each step when turning counter-clockwise. |

## State Values

| State                       | Definition                                                  |
| --------------------------- | -------------------------------------------------------- |
| `rotation_clockwise`        | number of times/clicks the encoder was turned clockwise. |
| `rotation_counterclockwise` | same.                                                    |

# EncoderPush

`type: encoder-push`

An EncoderPush is the combination of an Encoder and a push button.

## Attributes

| Attribute  | Definition                                                                                                                                                                                                                           |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `commands` | An EncoderPush has 3 commands:<br><br>1. First command gets executed when it is pushed<br>2. Second command gets executed when encoder is turned clockwise<br>3. Third command gets executed when encoder is turned counterclockwise |

## State Values

| State                       | Definition                                                  |
| --------------------------- | -------------------------------------------------------- |
| `rotation_clockwise`        | number of times/clicks the encoder was turned clockwise. |
| `rotation_counterclockwise` | same.                                                    |

# EncoderOnOff

`type: encoder-push`

An EncoderOnOff is the combination of an Encoder and an OnOff button.

## Attributes

| Attribute  | Definition                                                                                                                                                                                                                                                                                                        |
| ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `commands` | An EncoderPush has 4 commands:<br><br>1. First command gets executed when it is OFF, to turn it ON<br>2. Second command gets executed when it is ON, to turn it OFF<br>3. Third command gets executed when encoder is turned clockwise<br>4. Fourth command gets executed when encoder is turned counterclockwise |

## Options

| Attribute | Definition                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `dual`    | With option `dual`, the activation uses two more commands.<br><br>1. First command gets executed when it is OFF, to turn it ON<br>2. Second command gets executed when it is ON, to turn it OFF<br>3. Third command gets executed when encoder is turned clockwise and ON<br>4. Fourth command gets executed when encoder is turned counterclockwise and ON<br>5. Third command gets executed when encoder is turned clockwise and OFF<br>6. Fourth command gets executed when encoder is turned counterclockwise and OFF. |

## State Values

| State                       | Definition                                               |
| --------------------------- | -------------------------------------------------------- |
| `rotation_clockwise`        | number of times/clicks the encoder was turned clockwise. |
| `rotation_counterclockwise` | same.                                                    |
| `on`                        | Is currently On or Off                                   |

# EncoderLongPush

## Attributes

| Attribute  | Definition                                                                                                                                                                                                                                                                                                                                                    |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `commands` | An EncoderLongPush has 4 commands:<br><br>1. First command  gets executed when encoder is turned clockwise<br>2. Second command gets executed when encoder is turned counterclockwise<br>3. Third command gets executed when encoder is turned clockwise *and pushed*<br>4. Fourth command gets executed when encoder is turned counterclockwise *and pushed* |

## State Values

| State                       | Definition                                                  |
| --------------------------- | -------------------------------------------------------- |
| `rotation_clockwise`        | number of times/clicks the encoder was turned clockwise. |
| `rotation_counterclockwise` | same.                                                    |

# EncoderValue

`type: encoder-push`

An EncoderValue is an Encoder that increases or decrease an internal value each time it is rotated clockwise or counterclockwise. The value can be written to an X-Plane dataref or used for other pruposes.

## Attributes

| Attribute       | Definition                                                                                                                                                   |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `initial-value` | Initial value. Default to 0.                                                                                                                                 |
| `step`          | Amount of value increase or decrease.                                                                                                                        |
| `stepxl`        | Alternate value for step increase or decrease. If the encoder is capable of push action, the push action will switch between the `step` and `stepxl` values. |
| `min`           | Minimal value.                                                                                                                                               |
| `max`           | Maximal value.                                                                                                                                               |
| `formula`       | Optional. Formula to transform the `${state:button-value}` *before* it is sent to the dataref.                                                               |
| `set-dataref`   | Optional dataref to set to the value of the computed value. The value is sent right away, after each encoder activation.                                     |

## State Values

| State                       | Definition                                               |
| --------------------------- | -------------------------------------------------------- |
| `rotation_clockwise`        | number of times/clicks the encoder was turned clockwise. |
| `rotation_counterclockwise` | same.                                                    |
| `value`                     | Current raw value                                        |

# Slider

`type: slider`

A Slider is a one dimensional cursor that produces a continuous value within a range. The value can be written to an X-Plane dataref, directly, or after a computation.

## Attributes

| Attribute     | Definition                                                                                                                    |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `set-dataref` | Optional. Dataref to write the value to, if present.                                                                          |
| `formula`     | Optional. Formula to transform the `${state:button-value}` (value produced by the slider) *before* it is sent to the dataref. |

## State Values

| State   | Definition        |
| ------- | ----------------- |
| `value` | Current raw value |

# Swipe

`type: swipe`

A Swipe is a 2 dimensional movement of a finger on a surface. The event produced consists of the start and end positions of the finger relative to the surface (x, y) and timing information (time stamp).

(Currently not used.)

The Swipe event has no attribute.

## State Values

The event of a swipe is complex. The entire event is available as `last_event`.

The event has the following structure:

```yaml
ts_start: 123.456
ts_end: 135.246
pos_start: (23, 48)
pos_end: (78, 42)
```

## State Values

| State      | Definition |
| ---------- | ---------- |
| `start_x`  |            |
| `start_y`  |            |
| `start_ts` |            |
| `end_x`    |            |
| `end_y`    |            |
| `end_ts`   |            |

# Button Activations for Developers

Reload, Stop, or Inspect are [[Button Activations for Developers|special activations for developer]].

These activations are normally not used during regular operations.

# Activation Validity

Each Activation has a `is_valid()` method that checks whether all necessary attributes or parameters are available to it. The inspect keyword to trigger button validity inspection is `valid`.

Each Activation also has a `describe()` method that displays and explains what it does. The inspect keyword for displaying activation description is `desc`.

# New Activations

It always is possible to create new activations by [[Extending Cockpitdecks|extending Cockpitdecks]].
