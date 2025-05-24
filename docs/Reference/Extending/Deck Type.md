A Deck Type is a small file that details the available features of a deck device.

Some deck device have buttons, knobs, dials, some have slider or cursors, some have small screen for display of icons, some have just LED, sometimes coloured. The purpose of the Deck Type is to enumerates the features of the deck device to Cockpitdecks.

# Buttons

Different decks propose different means of interaction like keys to be pressed, dials, cursor, or sometimes even small touch screens. Inside Cockpitdecks, the generic term to designate these interaction means is the *Button*.

A *button* is the general term for a key, knob, rotary encoder, slider cursor, or touch surface on a deck. On a given deck, each element that can be pressed, turned, swiped, or slid is a *button*.

So a deck mainly consists of a collection of *buttons*. Describing a deck consists therefore in enumerating all available *buttons* on that deck with the particularities of each *button*.

## Button Identification

Cockpitdecks will need to individually address each button on a deck. To do so, each button on a deck will have a unique name, called its *index* on the deck.

Each button can either be named explicitly, or, if a deck has a row or an array of similar buttons, collectively with an numbered pattern.

To name a button explicatively, use the `name` attribute:

```yaml
name: touchscreen
```

To name a series of buttons collectively, use the `name` and `prefix` attributes:

```yaml
name: 4
prefix: key
```

The above naming will create buttons named `key4`, `key5`, etc. for as many button as required for the serie of identifical buttons.

## Button Repetition

If a deck contains a serie of identical buttons, the `repeat` attribute will tell how many buttons are available. The repeat attribute is 2 dimensional, width and height:

```yaml
repeat: [4, 3]
```

The above repetition state that the deck offers a replication of an identical button across 3 rows of 4 buttons.

If `repeat` attribute is specified, the identification of the button must use the `name` and `prefix` attributes.

## Button Description

Each button mainly has two key characteristics:

1. How is it manipulated? Is it pressed? Turned? Swiped?
2. What feedback means does it provide (as an individual button): A small LCD/TFT screen, one or more LED, may be colored LED, or feedback mean at all?

### Interaction: Action

Possible interaction means with a deck button is given through the `action` attribute:

```yaml
action: push
```

The `action` attribute consist of one or more possible interaction means called actions.

```yaml
action: [push, encoder]
```

The above attribute states that the button is capable of being pushed and turned with encoder steps.

Possible interaction means are (in other words,  possible values for the `action` attribute):

| `action`  | Description                                                                                                                                                                                     |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `none`    | There is no interaction with the button. It is only used for display purpose. (`none` interaction can be omitted.)                                                                              |
| `press`   | Simple press button that only reports when it is pressed (one event)                                                                                                                            |
| `push`    | Press button that reports 2 events, when it is pushed, and when it is released. This optionally allow for "long press" events.                                                                  |
| `swipe`   | A surface swipe event, with a starting touch and a raise events.                                                                                                                                |
| `encoder` | A rotating dial, that can turn both clockwise and counterclockwise. Each small rotation of the dial produces a "click" (encoder) and reports the direction in which the dial was turned.        |
| `cursor`  | A cursor is either a round dial or a linear handle that produces a countinuous value between two limits. The produced value changes when the dial is turned or the handle moved along its axis. |

### Feedback

Similarly, the `feedback`attribute is used to communication which feedback means an individual button offers to the user.

```yaml
feedback: image
```

Opposite to the `action` attribute that allows for one or more interaction means, the `feedback` attribute must specify at most one feedback mean.

Possible values for the `feedback`attribute are:

| `feedback`     | Description                                                                                                            |
| -------------- | ---------------------------------------------------------------------------------------------------------------------- |
| `none`         | No feedback on device, or direct feedback provided by some marks on the deck device. (`none` feedback can be omitted.) |
| `image`        | LCD/TFT image, often small and iconic sometimes even reduced in display color capacity.                                |
| `led`          | On/Off LED light.                                                                                                      |
| `colored-led`  | A single LED that can be colored, or off.                                                                              |
| `multi-leds`   | Several monochrome LED on a ramp.                                                                                      |
| `encoder-leds` | Special encoder led ramp for X-Touch Mini (4 modes)                                                                    |
| `vibrate`      | Emit a buzzer sound.                                                                                                   |
| `sound`        | Capable of playing a sound, or even music (speaker).                                                                   |

# Button Definition

A button on a deck is described by a list of attributes. Here are some exemples:

#### Single key button with LED light

```yaml
name: 4
action: push
feedback: LED
```

#### Single Encoder Dial

```yaml
name: enc0
action: [push, encoder]
feedback: none
```

#### «Array» of Idenfical Keys

```yaml
name: 0
prefix: key
action: push
feedback: image
image: [128, 128]
```

#### Touch Screen

```yaml
name: touchscreen
action: swipe
feedback: image
```

The description of a button on a deck is called a *Button Type Block*.

# Deck Buttons

A deck often consists of more than one button: Keys are combined with encoders, touchscreens, etc. Therefore the description of the deck consists of not one button definition but a list of button definition.

```yaml
name: LoupedeckLive
driver: loupedeck
buttons:
  - name: 0
    repeat:
      - 4
      - 3
    action: push
    feedback: image
    dimension:
      - 90
      - 90
  - name: 0
    prefix: e
    repeat:
      - 1
      - 3
    action:
      - encoder
      - push
  - name: 3
    prefix: e
    repeat:
      - 1
      - 3
    action:
      - encoder
      - push
  - name: 0
    prefix: b
    repeat:
      - 8
      - 1
    action: push
    feedback: colored-led
  - name: left
    action:
      - push
      - swipe
    feedback: image
    dimension:
      - 60
      - 270
  - name: right
    action:
      - push
      - swipe
    feedback: image
    dimension:
      - 60
      - 270
  - name: buzzer
    feedback: vibrate

```

This is the Deck Type definition of the LoupedeckLive deck.

# Deck Attributes

As seen above, the Deck Type also contains a few additional attributes necessary to identify the deck type, and its components (driver).

| Attribute | Definition                                                    |
| --------- | ------------------------------------------------------------- |
| `name`    | Name used inside Cockpitdecks to identifying the deck model.  |
| `driver`  | Keyword identifying the deck software driver class.           |
| `buttons` | List of button descriptions (list of Button Type Blocks).<br> |

# Correspondance

When creating an [[Adding Activations|Activation]], the activation will specify which **action** it requires. For example, an activation that requires an encoder dial to work will require the `encoder` or `encoder-push` action on the button.

Similarly, a [[Adding Representations|Representation]] will specify which **feedback** it requires. A representation that displays an image (icon, drawing, animation...) will require the `image` feedback in the button definition.

# See Also

[[Deck Type for Web Deck]]
