# ColoredLed

ColoredLed buttons are color capable led behind a transparent or mask button.
They are either on or off, with a RGB color.

```
colored-led: blue
```

# EncoderLEDs

`led: encoder-leds`

(Specific to the X-Touch Mini Encoders.)

MultiLeds are LED-based display that use more than one LED for reporting information.

X-Touch Mini encoders, for example, are surrounded by 11 LED that can be lit individually.

![[enc-status.png]]

## Attributes

`led-mode: fan` or `led-mode: 2`; name or number

Valid modes are:

0. Single
1. Trim
2. Fan
3. Spread

The value of the button determine how many leds will be displayed (0 to 11).

> [!NOTE] X-Touch Mini MACKIE MODE
> To send feedback instruction to the deck, two modes of interactions are available: Direct mode, and Mackie Mode. Cockpitdecks uses Mackie Mode which makes deck interaction easier and standard through the MIDI protocol. However, in Mackie Mode, only 11 or the 13 available encoder LEDs are accessible. It is not possible to access LED 0 and 13, only 1 to 12.

# IconSide

`icon-side: ICON_FILE_NAME`

An IconSide is a special icon for LoupedeckLive devices, used on either side of the main panel. IconSide have particular display capabilities to cope with their specifics, sizes, and their positions that allow to display information regarding the nearby encoders.

## Attributes

`labels`

Labels that are displayed on the icon.

`label-positions`

Label anchor position expressed in percentage of the 100% height of the side image.

Note: There might be similar icons to control the display of other, larger display like the Streamdeck Plus bottom LCD display.

# Larger Displays

Some decks exhibit displays that are larger than a key with and iconic representation. Those displays often have touch and swipe capabilities. This led to design activations and representations specific to these larger displays to be used with your favourite flight simulation software.

> Please note, for example, that the LoupedeckLive deck has a unique central display screen accepting touches and swipes. A plastic grid cover gives the illusion that there are two vertical side screens and 12 « keys » in the middle, but underlying is a unique 480 × 360 pixel screen. Each key is a 90 × 90 pixel portion of that screen.

## Usage

Larger screens can be used in two different ways:

- As a whole, unique, larger display surface,
- As a set of adjacent buttons covering the entire surface.

In the later case, the specificity of the display is not apparent, and resulting buttons are treated as regular keys with iconic image display (with different sizes sometimes.)

When considered as a single larger display, it is difficult to remain generic since each display will have special size, location, and behavior. Cockpitdecks buttons assigned to those specific display are therefore also very specific.

## Activations

Having no activation at all to use those display solely for displaying purposes is always an option. However simple activations can make the passive display a lot more enjoyable.

For exemple, if the display is capable, touching or swiping the display can be used to change its content. On startup, display heading, swipe left, display speed, swipe right display heading, etc.

## Touch

Touch is similar to button press activation and can be used as such.

## Swipe

Swipe returns movement start and end position and timing. These values can be interpreted to model a limited set of movements like swipe left, right or up, down. Raw values can also be used as an increasing or decreasing sliding cursor.

## Representations

Given the sizes of each display, representations fitting those displays will always remain very specific.

### Larger Horizontal Displays

#### Airbus Flight Control Unit

The highly specific Airbus FCU representation reproduces the central display with all possible modes.

#### Airbus Flight Mode Annunciators

The Airbus FMA displays the five annunciators on top of the Primary Flight Display (PFD).

There are two modes of display. The first one make use of a larger, horizontal display and shows all five annunciators next to each other. The second one use five keys with iconic display and show one annunciator on one key. In both cases, data necessary for display is fetched only once, the first annunciator being the master one with all data, other annunciators are slave ones and fetch their data from the master annunciator.

Airbus FMA display is a pure display with no activation associated with it.

### Larger Vertical Displays

On LoupedeckLive decks, vertical displays exploit their proximity to the lateral encoder dials to present direct encoder feedback. For example, next to the QNH adjustment encoder, the current atmospheric pressure is displayed. Pushing the encode switches between Standard pressure and local ambiant pressure.

### Icons

Image or drawn icons (especially text messages) are also an alternative way to decorate those displays. One can imagine displaying ATC instructions on a tape display, with swipe actions to acknowledge messages.
