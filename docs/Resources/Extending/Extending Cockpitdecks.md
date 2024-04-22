
Cockpitdecks can be extended in several directions.

First, new deck models may appear. [[New Deck|Adding a new deck model]] highlights steps necessary to add a new deck model.

Second, in addition of defining a new deck model, it might me necessary to [[New Interactions|develop specialized, custom interaction mechanism]] not foreseen by the current Cockpitdecks version.

If the new deck model brings a new way of interacting with it, it might be necessary to add a new method of interaction, a new *Activation*.

Similarly, if the new deck model brings a new feedback mechanism, it might be necessary to add a new method to bring the feedback to the user, a new *Representation*.

A Cockpitdecks developer may add a new type of feedback, very much like the [[Weather]] buttons. Those buttons do not exist in aircrafts, they were created to artificially enhance the simulation experience and ease other parts of the simulation. Similarly, there are endless opportunities to create other types of buttons and materialize them on a deck though Cockpitdecks. In Cockpitdecks, this is performed by mainly creating new *Representations*.


As a last resort, core classes such as

- Cockpit,
- Deck,
- Layout,
- Page, or
- Button

can also be extended if necessary, although this has not been formally designed.

Finally, core class [[Simulator]] can also be extended to connect Cockpitdecks to another simulation software. In this case, interaction models imposed by Cockpitdecks (datarefs, commands, UDP connection) inspired by the X-Plane flight simulator software may have to be adjusted as well.