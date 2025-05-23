# Deck

A Deck is a piece of hardware connected to a computer to provide some input by means of buttons, encoders, cursors... A Deck also provide some feedback to the user through colored LED lights, small LCD icon screens, or by emitting sound and vibration.

# Button

A button is a piece of a deck a user can use to interact with it.

It can be a simple button to press, a small LCD button, an encoder to turn, or a touch screen that can be swiped with one or multiple fingers.

# Web Deck

A Web Deck is an emulation of a deck in an internet browser window. Cockpitdecks provides a set of web decks that emulate existing decks.

Originally, web decks where created to speed up deck page creation and arrangement. But web decks turned out to be so convenient that they are now part of Cockpitdecks. Some users actually only use web decks, without any physical ones.

It is possible to design as create any web deck one can imagine, built from basic components like buttons, encoders, led, and LCD displays. As a proof of this concept, there is a entire overhead panel, fully reactive and responsive to the simulator changes.

Cockpitdecks allow a designer to create and use any web deck, in the context of the Cockpitdecks application.

Formerly, *Web Decks* were called *Virtual Decks*, in the context of Cockpitdecks, both names can be used interchangeably.

# Page

A Page is a collection of buttons that are displayed on a deck at a moment.

A deck can display different pages of buttons. A button on a page can be used to load another page.

All pages that can be displayed on a deck are grouped in a folder called the *Layout* of the deck.

# Layout

A Layout is a collection of pages that can be displayed on a given deck. On startup, a deck must tell Cockpitdecks which Layout it is going to use.

# Cockpitdecks

Cockpitdecks is the name of the application used to control decks connected to a computer. It is started with the command `cockpitdecks-cli`.

# Cockpit

The Cockpit is the main entity of Cockpitdecks that controls most parts of the application, from the start to its termination.

# Activation

An Activation is an action that is executed when interaction occurs on a deck.

# Representation

A Representation is an abstraction of an instruction sent to a deck to change its appearance. It can be turning a LED light on or off, playing a sound, or dynamically generating an iconic image and sending it for display on a deck LCD key.

# Variable

A Variable is a typed value. A Variable maintains a list of *Variable Listeners* that get notified whenever the value of the variable changes. There are mainly two types of Variables:

- Internal Variables are variables defined and used inside Cockpitdecks
- Simulator Variables are variables that represent a value inside the connected simulator. When the value of the variable changes in the simulation software, all listener of that variable get notified of the change.

## StringWithVariables

A StringWithVariables is a (string) variable. Its value contains one or more variables that get substitued when fetching its value.

## Formula

A Formula is a StringWithVariables. In the case of the formula, the value of the StringWithVariables is a Reverse-Polish Notation expression that further gets evaluated. The result of this evaluation is a numeric value, hence, the value of a Formula is a number.

## Value

A Value is a variable with elements associated with it, like a format (font, size, color…).

# Activity

An *Activity* is a fact that occurred in the simulation software and that is communicated to Cockpitdecks.

For example, X-Plane allows for command to report when they are activated. X-Plane reports such activation to Cockpitdecks through the *Activity* entity.

Other simulation software have other means to report activity that occurs when used. For example, MS Flight Simulator can report when a button in the cockpit user interface is pressed. There events all lead to the report of an *Activity* to Cockpitdecks.

The word Activity has been chosen to distinguish from events, actions, and activations.

# Event

An Event is a structure internal to Cockpitdecks that is created to notify that something occurred. Events are handled (processed) by Cockpitdecks.

## Simulator Event

Simulator events are either sent by the simulator software or created by Cockpitdecks when something occurred in the simulator. There are two major types of Simulator Events:

1. An *Activity* has been triggered inside the simulation software.
2. A value has changed.

## Deck Event

Deck Events are produced by deck driver software to notify Cockpitdecks that interaction occurred on the deck: A button was pressed, à encoder was turned, a cursor was slid…

# Instruction

An Instruction is a request emitted by Cockpitdecks to perform an action.  It is expressed through a little structure that contains the name of the instruction to carry, an optional delay to wait before the instruction is carried over, and an optional condition to satisfy before the instruction is carried over.

## Simulator Instruction

A Simulator Instruction is an order sent to the simulator to alter its behavior. There are two major types of instructions.

### Command

A Command is an instruction to perform an action.

#### Long Press

A Long Press is an instruction that needs to be carried over while/as long as a button is pressed. It is a variant of a command.

### Update Value of a Simulator Variable

The other type of Simulator Instruction is a request to update a value made accessible by the simulator.

## Cockpitdecks Instruction

Cockpit or Cockpitdecks Instructions are instructions triggered and used inside Cockpitdecks and are not forwarded to the simulator. Cockpitdecks instructions are:

- Change/load alternate deck page
- Reload decks
- Stop Cockpitdecks
- Load new aircraft configuration
- …

## « Macro-»Instruction

A Macro Instruction is an Instruction that consists of set of two or more instructions. Whenever an instruction is required, it is possible to use a Macro Instruction and trigger not one instruction but a whole set of instructions. Each individual instruction in the set is defined in an *instruction block*.

# See Also

[[History]]
