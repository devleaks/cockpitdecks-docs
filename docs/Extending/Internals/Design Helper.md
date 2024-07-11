Cockpitdecks comes with two designer tools.

1. A Web Deck Designer
2. A Button Designer

# Web Deck Designer

The Deck Designer is a tool that help positioning, sizing, and naming elements on a web deck background image.

![[deckdesigner.png|600]]

On the above designer, rectangular or circular buttons, encoder, or hardware image place holders can be positioned and sized.

A layout can be saved and loaded later. When saving, two files are created, one for the graphical representation with the design, and one Yaml file in Deck Type format. The latter can be used as a skeleton to define a new virtual web deck.

> [!WARNING] Layout and layout.
> In a very unfortunate choice of vocabulary, the word layout is used to designed two different things in Cockpitdecks.
> 1. [[Layout|Common layouts]] are sets of pages. A deck must reference a layout, that is a deck must reference a set of pages it will load and propose to the user. A common layout is just a folder that contains pages to be loaded on a deck.
> 2. Web Deck layout are arrangement of buttons, encoders, and hardware images on the web page of a web deck. They are quickly created, saved, and edited with the Deck Designer tool. Look at the above image, Deck Designer allows the user to add, remove, resize different elements to interact with over the background image. This arrangement of elements is also awkwardly called a layout. Sorry. Fortunately, the second meaning of layout is less frequent, and only used by Web Deck designers.

The web deck can then be used with the same background image, and each button, encoder, or hardware image will be laid over the background at the very precise defined position and size.

Of course, manual tweaking of position and sizes is sometimes necessary, but the gross work of estimation is completed in fun time. For instance image size may need to be evenly rounded to the same value for all buttons for aesthetic layouts.

## Development Example

See the A321 Overhead Panel example. Deck Designer is used to define the very precise position of each annunciator or switch.

# Button Designer

The Button Designer is a simple tools that help test and preview button design.

A button can be defined, either through a form by filling a few intuitive fields, or by typing Yaml code directly in the code window. The design tool also can generate Yaml code from the form parameters values.

> [!WARNING] Form is not completed from code area
> The opposite is not true, when entering or adjusting code in the coding window, form elements are not updated.

![[buttondesigner.png|600]]

By pressing the render button, Cockpitdecks will generate a button image and make it available for display in the Button Designer preview window. If the simulator is running, the button will show the simulated values like they would appear on the real deck.

When "Saving..." the button, it is added to a file in the

```
< current-aircraft | deck-name >/deckconfig/<layout-name>/<page-name>.yaml
```

file at the index position mentioned in the first part of the form. (If no layout or page name is supplied int he form, acceptable default values are provided.)

So is it possible to design buttons one by one and add them after verification to the page file.

Once the page is complete, it can be copied over to the `deckconfig` folder of the aircraft.

## Notes

Selecting a new deck name or a new button index resets the forms.

To generate code, the code window must be empty. Pressing render while the code window is not empty has no effect.

Representations are added one at a time, some are not currently working.

What is working well, it the testing area: You can paste your button definition yaml code in the code text area and press render to get a preview of your button, data included if connected to a simulator.

# See Also

[[Workflow for Web Deck Design]]
