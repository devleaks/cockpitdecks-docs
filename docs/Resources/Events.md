
Events are entities created by decks to report which interaction occurred on the physical device.
Events are created by callback functions to enter Cockpitdecks processing.

Events have a type that identifies the interaction.

# Event

Base class for events.

## Attributes

`deck`: Related [[Deck|deck]]
`button`: Related button [[Button Index|index]]
`action`: [[Deck Internals#Action|Deck action name]]
`timestamp`: Time of creation of event.
`is_processed`: Whether event has run or not

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
