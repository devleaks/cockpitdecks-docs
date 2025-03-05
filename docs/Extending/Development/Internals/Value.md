Cockpitdecks introduced the concept of a Value.

A Value defines where it gets its value from, and make its value always available through the simple abstraction.

Every entity that can have a value uses this abstraction.

The value of a Button,

The value of a chart or sparkline,

The value of an Annunciator part.

It is a dynamic entity. It does not store any value. It just know where to gets its value from and gets it.

It is up to the entity that uses the value to keep a copy, see if it has changed, keep the last ten values… The Value abstraction only knows where and how to find its value when asked to provide it.

A Value can report information about its behavior, like for instance the list of datarefs that are necessary to compute its final value.

The Value entity performs necessary variable substitution, computations if there is a formula, and can also take care of writing the value to an X-Plane dataref.

# Value Attributes

Here is the list of attributes that are inspected by a Value to determine its value.

| Attribute         | Definition                                                                                                                            |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| `dataref`         | Single Dataref, monitored if coming from X-Plane                                                                                      |
| `formula`         | Reverse Polish Notation expression with reference to Cockpitdecks variables such as datarefs, internal datarefs, and internal states. |
| `set-dataref`     | Dataref where the value is written to                                                                                                 |
| `any-attribute`   | If an attribute is added, Cockpitdecks will look into this very particular attribute for variable substitution. See below.            |

## Additional Attribute

Cockpitdecks will look into known additional attributes for variable substitution. Additionaly, a button designer can ask that its particular attribute be scanned as well.

Here is a typical example of such variable scanning:

```yaml
  - index: 5
    name: SQUAWK00XX
    type: none
    formula: ${sim/cockpit/radios/transponder_code} 100 / floor
    text:
      text: ${formula}
      text-format: "{:02.0f}"
      text-font: 7-segment-display-extended.otf
      text-size: 50
      text-position: rm
      text-color: khaki
      text-bg-color: (40, 40, 40)

```

In the above example, attribute `text` will be scanned for more datarefs. The attribute that gets scanned always has the same name as the main attribute:

```yaml
text:
	text: "text to be scanned for ${datarefs}"
```

or

```yaml
any-attribute:
	any-attribute: "additional attribute to be scanned for ${datarefs}"
```

A Value will always look into the following attributes:

- Text
- Formula
- Annunciator parts
