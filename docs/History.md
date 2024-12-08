The project started from the multiplication of accessory devices. Some are very dedicated and allow for little flexibility. [mini•COCKPIT](https://www.minicockpit.com) devices fall into that category. Some other devices are very generic and allow for customisations. Stream decks, Loupedeck devices or other MIDI-based devices fall into this category.

I created a single software to allow for the second category of devices to be used with X-Plane. This software relies on conventions to cope with all options and particularities of those devices. That is what I tried to bring into this project.

The history of this project explains decisions and processes that were taken to cope with the diversity of devices.

A first step was clearly to abstract particularities of device. Pressing keys, rotating knobs or encoders, sliders, touch screens are all different means to interact with some devices. Little iconic display, colored buttons, or multi-led ramps are all different means to communicate feedback to the user. This leads to two abstractions for interaction input (what is called *Activation*) and feedback output (what is called *Representation*).

A second step was to isolate this software from all device particularities and specificities. This leads to abstractions such as Deck (make, models, *device drivers* to interact with the device) and Buttons (things a user interact with on a device). A simple abstract model describe the device capabilities and physical organisation (the *Deck*).

A final step was to isolate all interactions with flight simulation software: Issuing commands and monitoring data values coming from it. This is done in a collection of specialised Simulator, Dataref, and Command abstractions.

Other entities glue things together with concepts like the *Cockpit* which is a collection of one or more decks, and a *Page* that is a grouping of buttons represented at once on a deck.

I hope this piece of software will allow you to make a better desktop cockpit and make your flight simulation more enjoyable.

# See Also

[Bio on X-Plane Forum](https://forums.x-plane.org/index.php?/profile/1019089-marespi/&tab=field_core_pfield_13)

![[a2fs1.jpg|center]]

[[Journey to Cockpitdecks]]
