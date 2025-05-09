The following page describe the key components of Cockpitdecks and its vocabulary.

# Cockpitdecks Objects

Walking though Cockpitdecks main entities will get you familiar with Cockpitdecks vocabulary. Main objects have familiar names and do represent what their name designate.

## Deck

The core object of Cockpitdecks is the Deck. It can be a real, physical deck device like Stream Deck or LoupedeckLive, but it can also be the representation of a deck in a web page. In the later case, it is called a *Web Deck*. Web deck can either be a representation of an existing physical deck like Stream Deck or LoupedeckLive, or an imaginary one. Historically, Web Decks were also called *Virtual Decks*, names can be interchanged and have the same meaning.

A Deck usually has buttons that can be pressed, encoders that can be turned. They often also have either simple led that can be turned on or off, or small iconic LCD screens where some iconic image can be displayed.

The goal of Cockpitdecks is to animate these devices to use them with a flight simulation software.

## Page

A Page is a collection of buttons that are displayed on a deck and ready to be used.

### Layout

A Layout is a group of related pages, displayed alternatively on a deck. On a Deck, a button can be used to change/load another Page of buttons on the deck.

## Button

A *button* is the general term for a key, knob, rotary encoder, slider cursor, or even touch surface on a deck. On a given deck, each element that can be pressed, turned, swiped, or slid is a *button*.

### Activation

The activation of a button determine what it will do when pressed, turned or slid, when it is manipulated.

When this button is pressed, we instructifs the simulation software to lower the landing gear.

### Representation

The representation of a Button determine what the button will display on the deck device. This depends on the capabilities of the button on the deck: monochrome LED, image, colored led, vibration, sound...

When the landing gear is lowered, we turn this LED green.

## Cockpit

The Cockpit is where everything occurs *!*

The Cockpit is a container entity, the maestro that orchestrate the symphony.

Cockpitdecks proceeds by numerous autonomous pieces of program that do some very precise task independently of each other, like listening to what happens on a deck or in the simulation software, and the Cockpit is responsible to orchestrate all these pieces of program so that they work in harmony to timely display information on decks or issue commands to simulation software.

## Simulator

The Simulator is an entity that bridges Cockpitdecks and the flight simulation software.

## Configuration

Cockpitdecks configuration is a folder (always named `deckconfig`), organized into sub-folders, where the Cockpit will find all instructions to execute, both on the simulator side and on the deck side and what to display on the decks.

# Cockpitdecks « Internal » Objects

Internally, Cockpitdecks uses a few objects with functions easy to understand.

## Variable

A Variable is a named entity ready to contain some data, a value.

There are several types of Variable depending on the source of the value.

Cockpitdecks internal variable is an internal value only used by Cockpitdecks, to maintain an internal state for example.

Simulator variable is a value coming from the simulation software. Each time it changes in the simulation software, its value is updated in Cockpitdecks. Reciprocally, when Cockpitdecks modifies a Simulator Variable, the new value is sent to the simulation software if permitted.

### Variable Listener

Each variable has listeners associated with it. Each time the value of a Variable changes, the listeners are notified of the updated value and can perform some tasks on their own to take into account the new value.

### String With Variables

A StringWithVariable is a character string Variable that contains variables to be substituted.

```
text: Current pressure is ${sim/weather/region/pressure} hPa.
```

`sim/weather/region/pressure` is a (simulator) variable, it’s value will be fetch from the simulator and substituted in the string. `${…}` is a marker to frame variables to be substituted.

The value of a StringWithVariable is the string with all values substituted:

Current pressure is 1013 hPa.

### Formula

A Formula is an expression that uses Variables to compute a new value. It is a string variable, but after substitution of the values, it is evaluated as a RPN expression.

A Formula is a Variable, and its value result from the evaluation of its expression.

## Instruction

An Instruction is a named entity that designate something to do, an action to perform.

There are several types of instructions.

Some are internal and specific to Cockpitdecks like loading a new page of button on a deck, or loading a new aircraft on all decks.

Some other instructions are oriented towards the simulator software and designate an action to perform inside the simulation software: change a value, or send a command to raise the landing gears.

## Event

An Event is created each time somethings of interest to Cockpitdecks occurs. There mainly are two types of Events.

*Events that come from the decks*: Each time a button is pressed, an encoder is turned, a slider is slid, or a touch screen touched, a *Deck Event* gets generated by the deck and is sent to Cockpitdecks for interpretation. Cockpitdecks analyses the event and issue the necessary Instruction(s) (see above) to handle the event.

*Events that come from the simulation software*: Each time a value changes in the simulation software, or some simulation event occurs, a *Simulator Event* is generated and sent to Cockpitdecks for handling. Again, Cockpitdecks will interpret the event and issue the necessary Instruction(s) (see above) to reflect the value change on the deck(s).

![[deck-simulator.png|600]]

Please note that for the above objects, Variable, Instruction, and Event, there always is a Cockpitdecks «internal» version of the object, and a simulator version of the object.

# See Also

[[Glossary]].

The [[Cockpit]].
