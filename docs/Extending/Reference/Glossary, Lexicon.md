# Button

A button is a piece of a deck a user can use to provide some interaction.

It can be a simple button to press, a small LCD button, an encoder to turn, or a touch screen that can be swiped with multiple fingers.

# Deck

A Deck is a piece of hardware connected to a computer to provide some input by means of buttons, encoders, cursors... A Deck also provide some feedback to the user through colored LED lights, small LCD icon screens, or by emitting sound and vibration.

# Layout

In Cockpitdecks, a Layout is a collection of pages that can be displayed on a deck. ON startup, a deck must tell Cockpitdecks which Layout it is going to use.

# Cockpitdecks

Cockpitdecks is the name of the application used to control decks connected to a computer.

# Activation

# Representation

# Simulator

## Simulator Data

## Simulator Instruction

# Event

## Simulator Event

## Deck Event

# Cockpitdecks Internals

## Cockpitdecks Data

Cockpitdecks data are internal Cockpitdecks values mainly kept for statistical or introspection purposes. Button internal state is maintained inside a few Cockpitdecks internal data.

End-users and Cockpitdecks developer can define and use internal cockpitdecks data. They all behave like simulator data, propagating changes to their listeners.

## Cockpitdecks Instruction

Cockpitdecks instructions are instructions triggered and used inside Cockpitdecks and are not forwarded to the simulator. Cockpitdecks instructions are:

- Change/load alternate deck page
- Reload decks
- Stop Cockpitdecks
- ...

## Cockpitdecks Events

Cockpitdecks does not currently generate or process Cockpitdecks internal events.

# Dataref

A Dataref is a simulator data inside the X-Plane flight simulator.

# SimVar

A SimVar is a simulator data from Flight Simulator. There are several types of SimVar. Please refer to the [Flight Simulator SDK](https://docs.flightsimulator.com/html/Introduction/SDK_Overview.htm) for more information.
