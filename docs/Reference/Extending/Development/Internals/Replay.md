Interactions with Cockpidecks enters the application through Events. Deck Events arrive from the user interaction with decks,  Simulator Events arrive because of Simulator data modification. Both then follow the same execution path, performing actions and adjusting deck feedback.

In the process, these events can be captured and saved for a later, for a replay of the same events for example.

That's the purpose of the Replay feature in Cockpitdecks.

Each interaction that occurred during the session has been recorded and can be replayed. It can be submitted to Cockpitdecks in a similar way it was submitted when interactions with the deck occurred. Simulator interactions are recorded as well but never replayed, as we expect the Simulator to respond to user interaction in a similar fashion each time.

Replay optionally respect the original timing.
