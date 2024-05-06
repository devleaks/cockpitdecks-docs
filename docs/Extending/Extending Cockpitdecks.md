
Cockpitdecks can be extended in several directions.

# New Button Appearances

The more probable, more common extension will be [[Adding Representations|adding custom Representations]].

A Cockpitdecks developer may add a new type of feedback, very much like the [[Weather]] buttons. Those buttons do not exist in aircrafts, they were created to artificially enhance the simulation experience and ease other parts of the simulation. Similarly, there are endless opportunities to create other types of buttons and materialize them on a deck though Cockpitdecks. In Cockpitdecks, this is performed by mainly creating new *Representations*.

# New Deck Model, New Activations

From time to time, new deck models may appear. [[Adding New Deck Models|Adding a new deck model]] highlights steps necessary to add a new deck model.

In addition of defining a new deck model, it might me necessary to [[Adding Activations|develop specialized, custom activation mechanism]] not foreseen by the current Cockpitdecks version.

If the new deck model brings a new way of interacting with it, it might be necessary to add a new method of interaction, a new Action type.

# New Simulator Software

Core class [[Simulator]] can also be extended to connect Cockpitdecks to another simulation software. In this case, interaction models imposed by Cockpitdecks (datarefs, commands, UDP connection) inspired by the X-Plane flight simulator software may have to be adjusted as well.

# Tools for Developers

Cockpitdecks incudes a few [[Button Activations for Developers|developer specific activations]] that can be used to introspect Cockpitdecks while running.

