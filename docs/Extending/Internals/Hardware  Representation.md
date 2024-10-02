Hardware Representations only exist for web decks.

 When a web deck tries to reproduce on screen hardware elements only available on physical decks, it uses *hardware representation*.

 Let us take a very simple hardware representation: A translucent button with a colored led. The button has sign on it (letter, number, icon…)

![[hardware-representation.png]]

 For the physical deck, the deck device driver will handle the rendering of the feedback on that deck through a dedicated representation and its accompanying driver software.

 Cockpitdecks uses a hardware representation to draw the same button on a web deck. A hardware representation is nothing more than an image used to represent a button on a web deck.

 Very much like the physical representation, Cockpitdecks uses dedicated software to create the image, but once created, the image of the hardware button is sent to the web deck for drawing like any other image, supplying the image of course, but also its position and size.

 Since a hardware representation is an image, all hardware representations are in fact particular icons image and treated as such.

 In the case of our translucent button, all we need is a image of that button when it is not lit. When lit with a given color and/or intensity, the hardware representation software will overlay the button sign to give the illusion that it is lit as requested. The resulting image is sent to the web deck to give the illusion of the lit button.

### Important Note

Hardware representations are often specific to a deck model. Therefore the device driver of the corresponding deck model need to be installed to make some feature parameters available to the hardware representation.

Here is an example of the demo deck, with a X-Touch Mini hardware representation for the right most encoder (circular ramp of 13 monochrome leds). In the picture below; on the left side, the encoder is not represented because `xtouchmini` driver software is not installed. On the right side, it is correctly represented.

![[hr.png]]
