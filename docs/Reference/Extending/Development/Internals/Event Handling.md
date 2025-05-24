# Deck Events

It is the deck driver’s responsibility to create a Deck Event based on the input of the user. Deck Events have a type: Press, long press, encoder turn, slide…

Deck Events are then enqueued for processing by the Cockpit.

The Cockpit dequeues the event, and from information about the deck and the button that was activated, forward the event for *activation*.

# Activation

The activation receives the event, and from its type (press, long press, encoder turned clockwise…) determine which instruction need execution. The activation then execute the Instruction (`Instruction.execute()`.)

The activation then determine its *activation value* and stores it.

When the activation is completed, if the button relies on its activation value, its value is re-computed. If its value changes, the button asks to be rendered again.

# Rendering

Rendering first check whether the button value has changed. If it has not changed, it most of the time renders a cached copy of its representation to prevent recreating the representation. Otherwise, it creates the representation using the new button value(s) and caches it.

Rendering always perform some tasks, at most send a cached copy of the representation to the deck. A rendering can be requested by a deck that asks to refresh its page, without modifying any underlying button value.

# Simulator Events

There are two types of simulator events:

- A value has changed,
- Something happened (occurrence of an event, like execution of a command).

## Value Change

When a simulator value has changed, Cockpitdecks stores the new value and warns each value listener. How value listeners handle the change is dependent on the listener. Often, the listener is the button. In this case, the button re-computes its value and update its rendering accordingly.

## Simulator Event

When an event of interest occurs inside the simulator, the instruction(s) associated to that event are executed.
