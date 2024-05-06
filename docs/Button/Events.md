
Events are entities created by decks to report which interaction occurred on the physical device.
Events are created by callback functions to enter Cockpitdecks processing.

Events have a type that identifies the interaction.

# Event

Base class for events.

## Attributes

`deck`: Related [[Deck|deck]] where interaction occured.
`button`: Related button [[Button Index|index]].
`action`: [[Deck Internals#Action|Deck action name]].
`timestamp`: Time of creation of event.
`is_processed`: Whether event has run or not.

## Common Functions

### run(just_do_it: bool = False)

Run the event.
If just_do_it is False, enqueue the Event.
Otherwise, execute the activation.

### handling()

Mark the start of processing.

### handled()

Mark the end of processing.

# PushEvent

## Parameters

`pressed`: `True` or `False`, if key is pressed (`True`) or released (`False`)

PushEvents are in fact raised twice for an interaction. First when the button is pressed, and second when the button it released. It is a main differentiator of Press and LongPress Events.

# PressEvent

A Press event is a single event sent when a button is pressed. No event is sent when the button is released. It is therefore impossible to know how long the button was pressed.

In the case of PressEvent, we only know that the button was pressed for a short time, without being able to set or determine how long it was pressed.

# LongPressEvent

A LongPress event is a single event sent when a button is pressed for a long time, but the time it remained pressed is not defined or settable. No event is sent when the button is released. It is therefore impossible to know how long the button was pressed.

In the case of LongPressEvent, we only know that the button was pressed for a long time, without being able to set or determine how long it was pressed.

The long time event occurs experimentally after roughly 600 milliseconds. It is therefore safe to assume that for a LongPress event, the button remained pressed for at least 1 second.

# EncodeEvent

`clockwise`: `True` or `False`, if encoder is turned clockwise (`True`) or counter-clockwise (`False`)

# SlideEvent

`value`: Raw value of the slider (as produced by the driver)

# TouchEvent

`x`: x-position of the event (horizontal)
`y`: y-position of the event (vertical)
`start`: Timestamp of touch event

# SwipeEvent

A Swipe event is either an event on its own, or the combination of two Touch event.

In the latter case, the first Touch event is the start of the swipe.
The second one being the end of the swipe, and must be supplied the first event as an argument. In this case, the swipe() method will return a Swipe event that combine them both.

> [!WARNING] Streamdeck Touch and Swipe Events
> Please note that some deck models do no report timing information. In this case, the timing information is either added by Cockpitdecks or not available at it.
> In particular, Streamdeck decks have a particular Swipe event that always last at most less that a second, without any timing information. Just a start position, and an end position taken at most one second after the start event.
