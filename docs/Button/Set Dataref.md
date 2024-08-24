# Theory of Operations

The `set-dataref` attribute points at a writable dataref. After each activation of a button, the value of the button is computed and written to that dataref.

# Button Value

When a button uses or produces a single value, that value gets written to the `set-dataref`.

When a button produces more than one value (for Annunciators, buttons without representation, etc.), the set-dataref needs additional information to know which value to write. In the latter case, a `formula` is mandatory to select the appropriate value to send to X-Plane.
