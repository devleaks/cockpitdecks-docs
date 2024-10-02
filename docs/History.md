The project started from the multiplication of accessory devices. Some are very dedicated and allow for little flexibility. [miniCockpit](https://www.minicockpit.com) devices fall into that category. Some other devices are very generic and allow for customisations. Stream decks, Loupedeck devices or other MIDI-based devices fall into this category.

I created a single software to allow for the second category of devices to be used with X-Plane. This software relies on conventions to cope with all options and particularities of those devices. That is what I tried to bring into this project.

The history of this project explains decisions and processes that were taken to cope with the diversity of devices.

A first step was clearly to abstract particularities of devices. Pressing keys, rotating knobs or encoders, sliders, touch screens are all different means to interact with some devices. Little iconic display, colored buttons, or multi-led ramps are all different means to communicate feedback to the user. This leads to two abstractions for input (what is called *Activation*) and output (what is called *Representation*).

A second step was to isolate all interactions with X-Plane: Issuing commands and monitoring dataref values. This is done in a collection of Simulator, Dataref, and Command abstractions.

A final step was to isolate this software from all device particularities and specificities. This leads to abstractions such as Deck (make, models, *device drivers* to interact with the device) and Buttons (things a user interact with on a device). A simple abstract model describe the device capabilities and physical organisation (the *Deck*).

Other entities glue things together with concepts like the *Cockpit* with is a collection of one or more deck, and a *Page* that is a grouping of buttons represented at once on a deck.

I hope this piece of software will allow you to make a better desktop cockpit and make your flight simulation more enjoyable.
