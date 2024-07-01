Cockpitdecks comes with two button designer tools.

# Deck Designer

The Deck Designer is a tool that help positioning and sizing elements on a virtual web deck background image.

![[deckdesigner.png|600]]

On the above designer, rectangular or circular buttons, encoder, or hardware image place holders can be positioned and sized.

A layout can be saved and loaded later. When saving, two files are created, one for the graphical representation with the design, and one Yaml file in Deck Type format. The latter can be used as a skeleton to define a new virtual web deck.

The virtual deck can then be used with the same background image, and each button, encoder, or hardware image will be laid over the background at the very precise defined position and size.

Of course, manual tweaking of position and sizes is sometimes necessary, but the gross work of estimation is completed in fun time. For instance image size may need to be evenly rounded to the same value for aesthetic layouts.

## Development Example

See the A321 Overhead Panel example. Deck Designer is used to define the very precise position of each annunciator or switch.

# Button Designer

The Button Designer is a simple tools that help test and preview button design.

A button can be defined, either through a form by filling a few I TUI rive fields, or by typing Yaml code directly in the code window. The design tool also can generate Yaml code from the form parameters values.

The opposite is not true, when entering or adjusting code in the coding window, form elements are not updated.

![[buttondesigner.png|600]]

By pressing the render button, Cockpitdecks will generate a button image and make it available for display in the Button Designer preview window. If the simulator is running, the button will show the simulated values like they would appear on the real deck.

When "Saving..." the button, it is added to a file in the

```
deckconfig/deck-name/layout-name/page-name.yaml
```

file at the index position mentioned in the first part of the form. (If no layout or page name is supplied int he form, acceptable default values are provided.)

So is it possible to design buttons one by one and add them after verification to the page file.

Once the page is complete, it can be copied over to the `deckconfig` folder of the aircraft.

## Notes

Selecting a new deck name or a new button index resets the forms.

To generate code, the code window must be empty. Pressing render while the code window is not empty has no effect.

Representations are added one at a time, some are not currently working.

What is working well, it the testing area: You can paste your button definition yaml code in the code text area and press render to get a preview of your button, data included if connected to a simulator.

# Workflow

Ideally, X-Plane simulator should be running to get live response data.

Place a web deck image in the aircraft resources/decks/images folder.

Start Cockpitdecks. Head to the Deck Designer page and select the above image.

Add interactors like buttons, encoders, and hardware images. Save the layout.

Declare a new virtual deck in the aircraft deckconfig folder, create a layout with a single empty page.

Reload the decks, which will provoke the reload of virtual web decks as well.

In the web deck list, select the newly created deck it will open in a new window.

Using the button designer create and test a new button. Fill in properly layout, page name, and button index. Save the button.

Reload pages immediately preview the appearance on the new button.

It is now possible to adjust any of the layout, or button definition, save them, reload and use the button.

> [NOTE] Web Deck Definitions
> Web deck definitions are located either in the Cockpitdecks code or in the aircraft deckconfig folder. On page reloads, web decks definitions located in the code are **not** reloaded. Web deck definitions in the aircraft folder are reloaded.

As a first button, I always create a new Reload button that I place on the side so that I can reload the whole setup each time some files where saved. When happy with the final deck, I simply comment out the code of the reload button. You will need it later…

```
index: reload
type: reload
icon: reload-page-icon.png
```

(Using an external file watcher, it is possible to provoke a deck reload each time a file has been saved, using curl(1) to post a websocket request for page reload to Cockpitdecks.)

# A Word of Advise

Never ever forget that the goal of these designer tools is currently not to provide you with a final design ready to be used. One day may be. The above designer tools aim at providing you with skeleton files that contain data that is difficult to get or estimate, thereby removing numerous trial and errors attempts.

The goal of the Deck Designer is to supply deck image positions and sizes for all items that need displaying on a virtual web deck.

The goal of the Button Designer is twofold:

1. Help you test button design right away, viewing a button’s appearance without requiring numerous « deck reload page ».
2. Learning the button design options and capabilities without searching through the code.

I hope both tools reach their goal and help you design buttons faster for your decks.

One more thing…

Never ever forget enjoy flying with your home made set up.

If Cockpitdecks crashes or is not responding, just restart it. It will restart, reconnect, and reload necessary data. If failures are persistent, just drop us a mail with a description of the problem and `Cockpitdecks.log` file.

Now just go and take off for new adventures.
