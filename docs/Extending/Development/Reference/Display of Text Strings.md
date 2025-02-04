Text string is a frequent representation mean for buttons. Each button can have a label displayed and other textual information like

- Static text,
- Dynamic text with values,
- Annunciator «parts» with text optionally framed in a box.

These frequent displayed elements have been isolated in a common structure for display. The structure simply consists of a convention of text elements that are used together and combined to show the final text.

Example 1: Labels

```
    label: RELOAD
    label-size: 12
    label-position: cm
```

Example 2: Static text display

```
    text:
      text: ${fa:rotate-right}
      text-font: fontawesome.otf
      text-size: 80
      text-position: cm
      text-color: lime
```

# Text «Block» Structure

A text structure consists of a keyword and then the following attributes attached to it:

```
    <keyword>:
      <keyword>: ${fa:rotate-right}
      <keyword>-font: fontawesome.otf
      <keyword>-size: 80
      <keyword>-position: cm
      <keyword>-color: lime
```

In some circumstances, additional attributes are possible, depending on the specificity of the representation.

# Text

The text displayed is read in the attribute `<keyword>`. For labels, (keyword=label), the text is static. (A Representation can change the text in the button attributes.)

For other representations, it is possible to use a text expression that contains variables. The text expression is evaluated before display. In this case, the evaluation consists of variable value substitution.

```
    text:
      text: ${AirbusFBW/BatVolts[0]}
      text-font: Seven Segment.ttf
      text-size: 24
      text-color: white
      text-format: '{:2.1f}'
```

In the above example, the value of the simulator variable `AirbusFBW/BatVolts[0]` is first converted to string using the `text-format` attribute and the subsitued in the value of the text attribute i.e.:

```
    text:
	  text: 26.8
      text-font: Seven Segment.ttf
      text-size: 24
      text-color: white
```

## Formula

If the component that has a text representation also has a formula, it is possible to substitude the result of the formula in the text expression:

```
	- text: ${formula}
	  formula: ${sim/cockpit2/gauges/barometer_setting_in_hg_pilot} 2 roundn
	  text-format: "{:4.2f}"
	  text-size: 22
	  text-position: cm
	  text-color: lightyellow
	  text-font: Segment7Standard
```

The keyword `${formula}` is replaced with the value of the formula expression.
