Please read about [[Deck Type]] first.

Deck Types for Web Decks are regular Deck Types with additional information necessary to draw the deck in a web page.

# Web Deck Drawing

A Web Deck is reproduced in a web page in a navigator window. It mainly consist of

1. A background
2. Images laid over the background.

Without going into deep details, Cockpitdecks draws web deck elements using the pixel unit as the image sizing element.

# Background

The background of the web deck can either be an image or a uniform color. If nothing is supplied, a default light or dark grey color is used.

The background of the web deck can have one or two backgrounds, for "light" and "dark" modes.

The attribute `background` is used for defining a web deck background.

When using colors only, it is necessary to specify a size for the web deck. If no size is specified, a default size of 100 × 100 pixels is used. When using background image, the size of the image is used as the size of the web deck.

```yaml
background:
  color: "#FF0000"
  alt_color: "#0000FF"
  size: [500, 800]
```

The above defines a web deck of size 500 × 800 pixels, with a light background color of red, and a dark bacground color of blue (`#0000FF`). Values of color must be valid HTML/CSS color values. Attribute `alt_color` is optional. If it is not specified, there will be possibility to change light or dark mode.

When using colors only, it is mandatory to supply the web deck size through the `size` attribute.

```yaml
background:
  image: mcdu.png
  alternate: mcdu-night.png
```

The above defines a web deck with background image mcdu.png. That image will be used for light background. Attribute `alternate` defines another image that will be used for dark background. Attribute `alternate` is optional. If it is not specified, there will be possibility to change light or dark mode. Both images should be the same size. (There is no control of this.)

# «Overlays»

Overlays are images laid over the background. There are two types of overlays that can be laid over a web deck background image.

First, when a button has an image as a feedback mechanism, its image representation is sent to the web deck, sized according to the Deck Type definition and finally laid over the background at the supplied offset location. The representation of the button, its image, is sent to the web deck for display. It is the exact same image that is sent to the physical deck device, in case the web deck emulates an existing physical deck.

Second, some existing physical deck feedback mechanism cannot simply be reproduced on a web deck, but they can easily be drawn. As a precise exemple, let us take a simple LED light. The representation of the physical deck is probably a binary value that is sent to the device to turn the LED on or off. This representation is meaningless for a web deck. However, it is possible to draw two images of the LED, one when it is lit, one when it is not, and send that image to the web deck for display (at the proper location, with the proper size). This image, that only exist for web deck, and that represent a feedback mechanism of a real physical deck is called a "hardware" representation.

# Representation

Representations that can be forwarded to a Web Deck are:

1. Sounds, with certain limitations
2. Images

All representation that either produces a sound file or an image can be displayed on a Web Deck.

## Sounds

Sounds produced by representation can be forwarded to a Web Deck if the sound file format can be played by the browser. Common format like `wav` or `mp3` are supported, other format aer not. A warning is issued by Cockpitdecks if the file format cannot be played on a Web Deck.

## Images

All representations that produce an image can be forwarded to a web deck: Icon, multi-icons, all drawn representations like switches, annunciators, data and weather...

To do so, the web deck needs to know where it will place the image on the deck. Two additional parameters are necessary: `dimension` and `offset`. The `offset` attribute is placed in a `layout` attribute.

```yaml
- name: screen
  action:
  - push
  dimension:
  - 335
  - 310
  feedback: image
  layout:
    offset:
    - 76
    - 62
```

If the `dimension` attribute is a single scalar value, it is assumed that the button is round, and the scalar value is the radius of the button.

If an identical button is repeated, it is necessary to supply the additional `spacing` attribute that will give the space to leave horizontally and vertically between repreating buttons:

```yaml
- name: 1
  prefix: annunciator
  action:
  - push
  dimension:
  - 35
  - 14
  repeat:
  - 5
  - 1
  feedback: image
  layout:
    offset:
    - 117
    - 11
    spacing:
    - 15
    - 0
```

All sizes are in pixels.

# «Hardware» Representation

Some existing deck device buttons cannot be reproduced on web decks. For example, a button with a translucent sign with colored back light is driven on the deck by supplying a series of numeric values to set the color and intensity of the light. There numeric values cannot be forwarded to a Web Deck to produce the same effect on the web deck, since there is not such button.

The idea behind «Hardware» Representations it do draw such a button on an image using the same numeric values provided by the regular representation, and send the image for display on the web deck. An Hardware Representation is an alternate representation for a regular representation that does not produce an image to allow it to be rendered on a Web Deck.

![[hardware-representation.png|500]]

## Hardware Image

A Hardware Representation is an image. Like regular representation it needs to get the position and size of the image on the web deck background through the `offset` and `dimension` attribute.

```yaml
  - name: 0
    prefix: b
    repeat:
      - 8
      - 1
    action: push
    dimension: 24
    feedback: colored-led
    layout:
      offset:
        - 57
        - 392
      spacing:
        - 39
        - 0
      hardware:
        type: virtual-ll-coloredbutton
```

In addition, the `hardware` attribute supply additional data for drawing the representation.

The hardware attribute has the following attributes:

- `type` - Mandatory. Name of the hardware representation, i.e. the alternate representation for this button on a web deck.
- `empty-button` - Optional. Button definition for this button without value.

The `empty-button` attribute is used to create the skeleton of a dummy button that will be used to create the representation when no button uses it. It is only used in case a hardware representation has no button assigned to it, to create an hardware representation with no value.

```yaml
      hardware:
        type: virtual-xtm-encoderled
        empty-button:
          type: encoder-value
          encoder-leds: 0
```

The above example is used to define the LoupedeckLive row of eight colored backlit buttons at the bottom of the deck device.
