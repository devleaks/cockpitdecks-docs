# Button

A button is a piece of a deck a user can use to provide some interaction.

It can be a simple button to press, a small LCD button, an encoder to turn, or a touch screen that can be swiped with multiple fingers.

# Deck

A Deck is a piece of hardware connected to a computer to provide some input by means of buttons, encoders, cursors... A Deck also provide some feedback to the user through colored LED lights, small LCD icon screens, or by emitting sound and vibration.

# Layout

In Cockpitdecks, a Layout is a collection of pages that can be displayed on a deck. ON startup, a deck must tell Cockpitdecks which Layout it is going to use.

# Cockpitdecks

Cockpitdecks is the name of the application used to control decks connected to a computer.

# Cockpit

The Cockpit is the main entity of Cockpitdecks that controls most parts of the application, from the start to its termination.

# Activation

An Activation is an action that is executed when interaction occurs on a deck.

# Representation

A Representation is an abstraction of an instruction sent to a deck to change its appearance. It can be turning a LED light on or off, playing a sound, or dynamically generating an iconic image and sending it for display on a deck key.

# Simulator

## Simulator Data

A Simulator Data is a variable value kept and modified in the simulator. Cockpitdecks expressed interest in receiving a notification when the value of the variable changes. The variable is accessed by its name.

## Simulator Instruction

A Simulator Instruction is something the simulator software understand to perform an action. Cockpitdecks generate instructions and submit them to the simulation sofwtware for execution.

## Simulator Events

See below.

# Event

An Event is created to notify Cockpitdecks that something occurred.

## Simulator Event

Simulator events are either sent by the simulator software or created by Cockpitdecks when something occurred in the simulator. There are two major types of Simulator Events:

1. An action has been triggered inside the simulation software.
2. A value has changed.

## Deck Event

Deck Events are produced by deck driver software to notify Cockpitdecks that interaction occurred on the deck: A button was pressed, à encoder was turned, a cursor was slid…

# Instruction

An Instruction is a request emitted by Cockpitdecks to perform an action.

## Simulator Instruction

A Simulator Instruction is an order sent to the simulator to alter its behavior. There are two major types of instructions.

### Command

A Command is an instruction to perform an action.

This included instructions that simulate key press.

### Update Value of a Simulator Data

The other type of Simulator Instruction is a request to update a value made accessible by the simulator.

# Cockpitdecks Internals

The Maestro of Cockpitdecks is the Cockpit entity. Internal components are interchangeably called *Cockpitdecks* items or *Cockpit* items (word is shorter *!*).

## Cockpitdecks Data

Cockpit or Cockpitdecks Data is an internal Cockpitdecks value mainly kept for statistical or introspection purposes. Button internal state is maintained inside a few Cockpitdecks internal data.

End-users and Cockpitdecks developer can define and use internal cockpitdecks data. They all behave like simulator data, propagating changes to their listeners.

## Cockpitdecks Instruction

Cockpit or Cockpitdecks Instructions are instructions triggered and used inside Cockpitdecks and are not forwarded to the simulator. Cockpitdecks instructions are:

- Change/load alternate deck page
- Reload decks
- Stop Cockpitdecks
- Load new aircraft configuration
- ...

## Cockpitdecks Events

Cockpit or Cockpitdecks Events are generated internally by Cockpitdecks to notify interested parties that something occurred internally, like loading a new page, reloading the decks, or changing the decks because a change of aircraft was detected.

# Dataref

A Dataref is a simulator data inside the X-Plane flight simulator.

# SimVar

A SimVar is a simulator data from Flight Simulator. There are several types of SimVar. Please refer to the [Flight Simulator SDK](https://docs.flightsimulator.com/html/Introduction/SDK_Overview.htm) for more information.
