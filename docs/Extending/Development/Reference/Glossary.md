# Button

A button is a piece of a deck a user can use to provide some interaction.

It can be a simple button to press, a small LCD button, an encoder to turn, or a touch screen that can be swiped with multiple fingers.

# Deck

A Deck is a piece of hardware connected to a computer to provide some input by means of buttons, encoders, cursors... A Deck also provide some feedback to the user through colored LED lights, small LCD icon screens, or by emitting sound and vibration.

# Web Deck

A Web Deck is an emulation of a deck in an internet browser window. Cockpitdecks provides a set of web decks that emulate existing decks.

Originally, web decks where created to speed up deck page creation and arrangement. But web decks turned out to be so convenient that they are now part of Cockpitdecks. Some users actually only use web decks, without any physical ones.

But it is possible to design as create any web deck one can imagine, built from basic components like buttons, encoders, led, and LCD displays. As a proof of this concept, there a entire overhead panel, fully reactive and responsive to the simulator changes.

Cockpitdecks allow a designer to create and use any web deck, in the context of the Cockpitdecks application.

Formerly, *Web Decks* were called *Virtual Decks*, in the context of Cockpitdecks, both names can be used interchangeably.

# Layout

A Layout is a collection of pages that can be displayed on a deck. On startup, a deck must tell Cockpitdecks which Layout it is going to use.

# Page

A Page is a collection of buttons that are displayed on a deck at a moment.

A deck can display different pages of buttons. A button on a page can be used to switch or change a page, leading to a literally infinite number of pages.

All pages that can be displayed on a deck are grouped in a folder called the Layout of the deck.

# Cockpitdecks

Cockpitdecks is the name of the application used to control decks connected to a computer.

# Cockpit

The Cockpit is the main entity of Cockpitdecks that controls most parts of the application, from the start to its termination.

# Activation

An Activation is an action that is executed when interaction occurs on a deck.

# Representation

A Representation is an abstraction of an instruction sent to a deck to change its appearance. It can be turning a LED light on or off, playing a sound, or dynamically generating an iconic image and sending it for display on a deck key.

# Variable

A Variable is a typed value. A Variable maintains a list of "Variable Listener" entities that get notified whenever the value of the variable changes. There are mainly two types of Variables:

- Internal Variables are variables defined and used inside Cockpitdecks
- Simulator Variables are variables that represent a value inside the connected simulator.

# Simulator

## Simulator Variable

A Simulator Variable is a variable value kept and modified in the simulator. Cockpitdecks expressed interest in receiving a notification when the value of the variable changes. The variable is accessed by its name.

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

### Update Value of a Simulator Variable

The other type of Simulator Instruction is a request to update a value made accessible by the simulator.

### Long Press

A Long Press is an instruction that needs to be done while/as long as the button is pressed.

### View

A View is a command. Historically, a view was a second command that could be attached to an Instruction to perform a view change in the simulation software.

Instructions have evolved with time and view are no longer necessary or can now be expressed by other means. However, the terminology remained and view can still be declared in activations.

# « Macro-»Instruction

A Macro Instruction is an Instruction that consists of set of two or more instructions. Whenever an instruction is required, it is possible to use a Macro Instruction and trigger not one instruction but a whole set of instructions. Each individual instruction in the set is defined in an *instruction block*.

# Cockpitdecks Internals

The Maestro of Cockpitdecks is the Cockpit entity. Internal components are interchangeably called *Cockpitdecks* items or *Cockpit* items (word is shorter *!*).

## Cockpitdecks Internal Variable

 Internal Variable is a value kept for statistical or introspection purposes inside Cockpitdecks, or its components. A Button internal state is maintained inside a few Internal Variables.

End-users and Cockpitdecks developer can define and use internal variable. They all behave like any other Variable, propagating changes to their listeners.

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

A Dataref is a Simulator Variable inside the X-Plane flight simulator.

# Command

A Command is an Instruction sent to the X-Plane flight simulator.

# SimVar

A SimVar is a Simulator Variable from Microsoft Flight Simulator. There are several types of SimVar. Please refer to the [Flight Simulator SDK](https://docs.flightsimulator.com/html/Introduction/SDK_Overview.htm) for more information.

# See Also

[[History]]
