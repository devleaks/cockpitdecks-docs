Adding a new deck model can be very simple or very difficult, depending on the deck's capabilities and software already available to access it (through the python language).

Cockpitdecks does its best at isolating deck specifics into

- deck definition files (called a Deck Type)
- deck (device) drivers classes

# Deck Definition File

## Deck Abstraction and Modeling

Decks, in general, have been defined as devices containing buttons that can be pressed, dials, or encoders that can be turned, cursors, or sliders that can be slid, or even touch screens that can be touched or swiped.

These buttons can be illuminated by one or more monochrome or color LED(s), or can sometimes even display an image on a small LCD. Some can vibrate, or even produce sound.

Each of these interaction has led to the definition of standardized behavior. On one side, the deck expresses what is is capable of, on the other side, the Cockpitdecks designer tells which capabilities she or he wants to produce a result.

Please refer to the [[Reference/Extending/Deck|Deck]] document to learn about how to express deck specifics in a Deck Type definition file.

## Definition Files

A small file defines the deck layout and capabilities, what is available through it:

```yaml
# This is the description of a deck's capabilities for a Elgato Streamdeck Plus device
#
---
type: Streamdeck Plus
brand: Streamdeck
model: Plus
driver: streamdeck
buttons:
  - name: 0
    action: push
    feedback: image
    image: [96, 96, 0, 0]
    repeat: 8
  - name: 0
    prefix: e
    action: encoder-push
    feedback: none
    repeat: 4
  # touchscreen in streamdeck package vocabulary
  - name: touchscreen
    action: swipe
    feedback: image
    image: [800, 100, 0, 0]
```

Through this file, Cockpitdecks is capable to determine that there are 8 (repeat) LCD buttons, named `0`.. `7`, capable of being pushed (action), and capable of displaying a 96x96 pixel image (feedback, image). Similarly, there are 4 encoders and a swipe screen.

| Attribute | Definition                                                                                                                                                                                       |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| type      | Name the precise deck model. Referenced in config.yaml file to tell which deck is connected to the system.                                                                                       |
| driver    | Name of the driver software inside Cockpitdecks. See below.                                                                                                                                      |
| buttons   | Button capabilities are modeled in two categories:<br><br>1. Actions, which specifies what a button is capable of,<br>2. View, which specifies what a button can show as a feedback to the user. |

### Button Capabilties

| Attribute | Definition                                                                                                                                                                                                                                                                                                                    |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| action    | The following **actions** (means of interaction with a deck) have been identified and are available in Cockpitdecks:<br><br>- `push`: ability to press a button, optionally pushing a long time,<br>- `encoder`<br>- `press`: ability to press a button, no timing information<br>- `longpress`<br>- `touch`<br>- `swipe`<br> |
| feedback  | Similarly, decks defined the following **feedback** interactions:<br><br>- `image`<br>- `led`: simple on-off light<br>- `colored-led`: colored light (that can also be off)<br>- `encoder-led`: ramp of led lights for X-Touch Mini                                                                                           |
| name      | Name and repeat will determine the index name of the buttons.                                                                                                                                                                                                                                                                 |
| repeat    | Number of time the same button needs repeating                                                                                                                                                                                                                                                                                |
| prefix    | Prefix is used to distinguish button capabilities. If a button has, for example, encoder and push capabilities, the push capabilities will use name `0` (name only), the corresponding encoder will be named `e0` (prefix + name).                                                                                            |
| image     | In case of an `image` feedback, the image attribute sets the image size for this button, and its offset if the image is a portion of a larger display.                                                                                                                                                                        |
| vibrate   | If the device/button has a vibration capability                                                                                                                                                                                                                                                                               |
| sound     | If the device/button has a sound emission capability                                                                                                                                                                                                                                                                          |

# Deck « Driver »

## Deck to Computer Interaction

Depending on the button's action that is triggered, the deck will programmatically generate an Event. The type and content of the Event will depend on the action type.

`push`: produces an event with the identifier of the button that was pressed, and a flag indicating that the button was pressed or release.

`encoder`: will produce an Event of type encoder, with the identifier of the encoder, and a flag telling whether the encoder was turned clockwise or counter-clockwise.

`swipe`: will produce a complex swipe Event, with the position of the start of the swipe, the end of the swipe and some timing information (timestamps of start and end of swipe).

Technically speaking, the deck will start a thread to listen to incoming events. Interaction will be decoded (which key was pressed, when, how, etc.) and presented to Cockpitdecks as a typed Event.

Depending on the device driver's hardware access, events will either be presented directly to Cockpitdecks, or through a FIFO queue: Driver just enqueues events, Cockpitdecks dequeues events and does the work.

## Computer to Deck Interaction

Depending on the deck's view capabilities, the computer will send the appropriate information to the deck to produce the feedback: Send an image, turn a LED on or off, with the appropriate color, emit a sound or vibrate.

This is performed directly through the deck's device driver, by calling the appropriate function. Most of the time, this call is direct.

# Correspondance

When creating an Activation, the activation will specify which **action** it requires. For example, an activation that requires an encoder dial to work will require the `encoder` or `encoder-push` capability.

Similarly, a Representation will specify which **feedback** it requires. A representation that displays an image (icon, drawing, animation...) will require a `image` feedback for instance.

# See Also

[[Reference/Extending/Deck|Deck]]
