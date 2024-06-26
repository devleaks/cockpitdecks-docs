Cockpitdecks comes with two button designer tools.

# Deck Designer

The Deck Designer is a tool that help positioning and sizing elements on a virtual web deck background image.

![[deckdesigner.png|600]]

On the above designer, rectangular or circular buttons, encoder, or hardware image place holders can be positioned and sized.

A layout can be saved and loaded later.

Once saved, the "graphical" layout can be converted into a virtual deck layout.

The virtual deck can then be used with the same background image, and each button, encoder, or hardware image will be laid over the background.

Of course, manual tweaking of position and sizes is sometimes necessary, but the gross work of estimation is completed in fun time.

## Development Example

See the A321 Overhead Panel example.

# Button Designer

The Button Designer is a simple tools that help test and preview button design.

A button can be defined, either through a form or by typing Yaml code directly in the code window. The design tool also can generate Yaml code from the form parameters values.

(The opposite is not true, when entering or adjusting code in the coding window, form elements are not updated.)

![[buttondesigner.png|600]]

By pressing the render button, Cockpitdecks will generate a button image and make it available for display in the Button Designer, in the preview window. If the simulator is running, the button will show the simulated values.

When "Saving..." the button, it is added to a file in the

```
deckconfig/deck-name/layout-name/page-name.yaml
```

file at the index position mentioned in the first part of the form.

So is it possible to design buttons one by one and add them after verification to the page file.

Once the page is complete, it can be copied over to the `deckconfig` folder of the aircraft.

## Notes

Selecting a new deck name or a new button index resets the forms.

To generate code, the code window must be empty. Pressing render while the code window is not empty has no effect.

Representations are added one at a time, some are not currently working.

What is working well, it the testing area: You can paste your button definition yaml code in the code text area and press render to get a preview of your button, data included if connected to a simulator.
