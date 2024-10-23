Interactions with Cockpitdecks leads to creation of events that are submitted for execution.

In the process, these events can be saved for a later replay of the same events.

That's the purpose of the Replay feature in Cockpitdecks.

Replay optionally respect the original timing.

There are two main types of events in Cockpitdecks:

1. Events coming from the simulator (value changes).
2. Events coming from decks, events that are usually provoked by users interacting with the deck.
Deck events are the only events that are replayed. Simulator events are not replayed.
