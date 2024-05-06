The `index` attribute is a mandatory attribute of a Button. It designate a very precise button on the deck. Hence, it defines what the button is capable of performing and how it will be rendered.

# Indices for Streamdeck Devices

![[streamdeck.png]]

## Keys

Streamdeck Mk.2, XL, Mini are composed of square icon keys. Image sizes vary with models.

Streamdeck + (Plus) as 8 square keys, 4 knobs, and 1 larger LCD capable of touch interactions.

![[streamdeckplus.png]]

## Encoders

Streamdeck Plus encoders are aligned at the bottom of the deck and have indices e0 to e3.

## Touchscreen

Streamdeck Plus has a 800x100 pixel touchscreen between the keys and the encoders.

# Indices for Loupedeck Devices

![[loupedecklive.png]]

## Keys

LoupedeckLive has 6 knobs, 12 square keys (90x90), and 2 side LCD (60x270).

There is an additional row of 8 push button labeled 0 (dotted-circle) to 7.

Both LCD and all 12 keys form a larger (480x270) LCD capable  of touch interaction.

In an alternate presentation, side LCD can be considered as 3 individual buttons each.

To simplify, we assume in this case that all 6 side buttons are square (60x60). Space between these button is filled with default color, image, or pattern.

# Indices for Behringer Devices

![[xtouchmini.png]]

## Keys

XTouchMini has 8 encoders with 8 "multi-led" displays, and 16 buttons with LED.

There is a slider and two more buttons on the side. These two buttons were meant to be used as "page" switches between 2 pages labeled A and B. For simplicity, we chose to not change the use of these two buttons and they should be programmed as Page Loader activation. The currently loaded page can be highlighted, like on the above photograph, Page A is loaded.
