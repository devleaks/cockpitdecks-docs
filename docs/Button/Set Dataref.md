# Theory of Operations

The `set-dataref` attribute points at a writable dataref. After each activation of a button, the value of the button is computed and written to that dataref.

# Button Value

When a button uses or produces a single value, that value gets written to the `set-dataref`.

When a button produces more than one value (for Annunciators, buttons without representation, etc.), the set-dataref needs additional information to know which value to write. In the latter case, a `formula` is mandatory to select the appropriate value to send to X-Plane.

# Self-Modifying Button

In some case, the button is using the same dataref for representation and set-dataref.

Here is the simplest example of this case:

```
- index: 0
  type: push
  text:
    text: ${formula}
  formula: ${data:activation_count}
  set-dataref: data:internal_counter
```

The indent of the above button is to display a counter of how many times the button was pressed. In this example, it appears clearly that the order of operation must be:

1. The button is pressed, the activation is triggered.
2. The activation executes the push instruction AND INCREMENT the activation counter.
3. It is only after the counter that has been incremented that the formula is evaluated and the new value of the button is computed
4. The new value of the button is computed and saved in the dataref pointed by `set-dataref`.

So, in case the value of button is computed from one or more values that are very precisely modified by the activation of the button, the modification of the values is registered first, and the final value of the button computed afterwards, just before it is optionnaly written to the dataref pointed by the `set-dataref` attribute.