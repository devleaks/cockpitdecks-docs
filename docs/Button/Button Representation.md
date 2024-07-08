The Representation of a button determine how it will be displayed on the deck device.

The representation depends on the capabilities of the button on the deck. There is a list of valid representations or a given button on a deck. A image or icon cannot be displayed on a LED-only button.

In the button definitions, the presence of a *specific attribute* with determine how the button gets represented on the deck. The name of that attribute is the key word of the representation. For example:

- `icon`: To show a image image
- `led`: To turn a LED on or OFF
- `annunciator`: To display a complex image as an icon
- etc.

The first attribute mentioned in each section below determines the type of Representation (`icon`, `text`, `multi-icons`, etc.) If more than one representation is found, or a representation that is not valid for the given button, a warning message is reported and the button does not render anything.

If no Representation is found, a warning message is reported and the button is assumed having no representation. For example, a X-Touch Mini slider has no representation. To suppress the warning message, the `representation` attribute can be used and set to `false`.

```yaml
  - index: slider
    name: SLIDER
    type: slider
    representation: false
```

will not issue any warning message.

# Representations

Here is a list of currently available, general purpose representations.

## Basic Button Representations

- [[Basic#Icon]]
- [[Basic#MultiIcons]]
- [[Basic#IconText]]
- [[Basic#MultiTexts]]
- [[Basic#LED]]
- [[Basic#ColoredLED]]

## Drawn Representations

- [[Annunciator]]
- [[Drawn Buttons – Switches]]
	- [[Drawn Buttons – Switches#Switch]]
	- [[Drawn Buttons – Switches#Circular Switch]]
	- [[Drawn Buttons – Switches#Push Button and Knob]]
- [[Drawn Buttons|Other drawn representations]]
	- [[Drawn Buttons#Data]]
	- [[Drawn Buttons#Decor]]

## More complex Button Representations

- [[Animations]]
- [[Weather]]
- Other
	- Aircraft

## Deck Specific Displays and Representations

- [[Deck Specific#EncoderLEDs]]
- [[Deck Specific#IconSide]]

## Aircraft / Deck Specific Representations

(To be used as model for alternate development.)

- Toliss Airbus FMA Display
- Toliss Airbus FCU

# Common Representation Attributes

## Managed

`dataref: dataref-path`

Path to a dataref that is interpreted to determine whether the value is *managed*.

If the value is *managed*, the value can be displayed as a text string in an alternative way depending on the `text-alternate` value.

`text-alternate: dash=4`: Represent managed value by a set of `-`. Default is 3 dashes.

`text-alternate: dot`: Represent managed value by a single dot `•`.

```yaml hl_lines="12-14"
  - index: 0
    type: none
    name: FCU Airspeed display
    label: SPD
    text: ${sim/cockpit2/autopilot/airspeed_dial_kts_mach}
    text-format: "{:3.2f}"
    text-color: khaki
    text-size: 24
    text-font: Seven Segment.ttf
    text-position: cm
    text-bg-color: (40, 40, 40)
    managed:
        dataref: AirbusFBW/SPDmanaged
        text-alternate: dash
```

In example above, *speed managed* mode, if `AirbusFBW/SPDmanaged` dataref value is non zero, the text will display `---`. Otherwise, it will display the air speed.

## Guard

`dataref: dataref-path`

Path to a dataref that is interpreted to determine whether the button or key is *guarded* (protected against unintentional use by a cap or lock). If *guarded*, it can be displayed in an alternative way depending on the options value.

`type`: Protects the button with a full red cover (`type: full`) or a see-through grid (`type: grid`)(cover is the default).

`color`: Color of the guard. Default is red for cover, and translucent red for grid.

```yaml  hl_lines="2-5"
    label: RAM AIR
    guard:
      type: grid
      color: black
      dataref: ckpt/ramair/cover
      # 0=closed, 1=opened
```

Guarded buttons or keys need to be pressed twice to activate, the first activation lifts the guard, the second one acts normally. To replace the guard, a long press of more than 2 seconds is necessary to replace (close) the guard.

> [!WARNING] Long Press
> Make sure long press lasts 2 seconds or more, otherwise the button will be activated!

# Representation Validity

> [!NOTE] Development Notes
> For developer only

Each Representation has a `is_valid()` method that checks whether all necessary attributes or parameters are available to it and valid. If the validity function fails, a warning is reported and the button is not rendered. The inspect keyword used to verify the validity of the representation is `valid`.

Each Representation has an `describe()` method that explains what it displays in plain English. The inspect keyword used to describe the representation is `what`.
