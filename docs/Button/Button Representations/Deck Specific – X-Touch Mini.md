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
> To send feedback instruction to the deck, two modes of interactions are available: Direct mode, and Mackie Mode. Cockpitdecks uses Mackie Mode which makes deck interaction easier and standard through the MIDI protocol. However, in Mackie Mode, only 11 of the 13 available encoder LEDs are accessible. It is not possible to access LED 0 and 13, only 1 to 12.

## Value Mapping

If nothing is specified, raw button value is used.

If `value_min` and `value_max` are specified in the encoder attribute, the Encoder representation performs a linear mapping between (`value_min`, `value_max`) and the range of valid values (0, 10).

A Warning is issued if the value is out of the range 0-11.
