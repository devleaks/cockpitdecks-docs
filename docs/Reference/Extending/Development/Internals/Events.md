Events are entities created to report activity to Cockpitdecks.

Events have a type that identifies the interaction.

# Event

Base class for events.

## Attributes

| Attribute    | Definition                    |     |
| ------------ | ----------------------------- | --- |
| action       | Deck action name              |     |
| timestamp    | Time of creation of event     |     |
| is_processed | Whether event has run or not. |     |

## Common Functions

| Function                      | Description                                                                                                |
| ----------------------------- | ---------------------------------------------------------------------------------------------------------- |
| run(just_do_it: bool = False) | Run the event.<br><br>If just_do_it is False, enqueue the Event.<br><br>Otherwise, execute the activation. |
| handling()                    | Mark the start of processing.                                                                              |
| handled()                     | Mark the end of processing.                                                                                |

## See Also

- [[#Deck Event]]
- [[#SimulatorEvent]]

# Deck Event

Events are entities created by decks to report which interaction occurred on the physical device.

## Attributes

| Attribute | Definition                 |                                |
| --------- | -------------------------- | ------------------------------ |
| deck      | Related [[Reference/Deck]] | where interaction occured.     |
| button    | Related button *index*     | button that produced the event |

# PushEvent

## Parameters

| Attribute | Description                                                                   |
| --------- | ----------------------------------------------------------------------------- |
| pressed   | `True` or `False`, if key is pressed (`True`) or released (`False`)           |
| pulled    | True if push/pull option is enabled and button was pulled rather than pushed. |
|           |                                                                               |

PushEvents are in fact raised twice for an interaction. First when the button is pressed, and second when the button it released. It is a main differentiator of Press and LongPress Events.

# PressEvent

A Press event is a single event sent when a button is pressed. No event is sent when the button is released. It is therefore impossible to know how long the button was pressed.

In the case of PressEvent, we only know that the button was pressed for a short time, without being able to set or determine how long it was pressed.

# LongPressEvent

A LongPress event is a single event sent when a button is pressed for a long time, but the time it remained pressed is not defined or settable. No event is sent when the button is released. It is therefore impossible to know how long the button was pressed.

In the case of LongPressEvent, we only know that the button was pressed for a long time, without being able to set or determine how long it was pressed.

The long time event occurs experimentally after roughly 600 milliseconds. It is therefore safe to assume that for a LongPress event, the button remained pressed for at least 1 second.

# EncodeEvent

## Parameters

| Attribute | Description                                                                               |
| --------- | ----------------------------------------------------------------------------------------- |
| clockwise | `True` or `False`, if encoder is turned clockwise (`True`) or counter-clockwise (`False`) |

# SlideEvent

| Attribute | Description                                         |
| --------- | --------------------------------------------------- |
| value     | Raw value of the slider (as produced by the driver) |

# TouchEvent

| Attribute | Description                          |
| --------- | ------------------------------------ |
| x         | x-position of the event (horizontal) |
| y         | y-position of the event (vertical)   |
| start     | Timestamp of touch event             |

# SwipeEvent

A Swipe event is either an event on its own, or the combination of two Touch event.

In the latter case, the first Touch event is the start of the swipe.

The second one being the end of the swipe, and must be supplied the first event as an argument. In this case, the swipe() method will return a Swipe event that combine them both.

> [!WARNING] Streamdeck Touch and Swipe Events
> Please note that some deck models do no report timing information. In this case, the timing information is either added by Cockpitdecks or not available at it.
> In particular, Streamdeck decks have a particular Swipe event that always last at most less that a second, without any timing information. Just a start position, and an end position taken at most one second after the start event.

# SimulatorEvent

A Simulator Event is created when something occurred in the simulator.

For X-Plane simulator, The following events are reported:

1. The value of a monitored simulator variable changed. The event contains the new value.
2. A command that is being monitored was activated in the simulator.

## DatarefEvent

Dataref path

Dataref new value

## CommandActiveEvent

Command path

Active on or off
