Toliss Airbus Aircrafts A319, A320, A321, and A340 share numerous functions, with particularities or specialties. They share a common set of functions, displays, or dataref values. This has led to the development of very specific buttons representation.

- Flight Control Unit display (in both horizontal and vertical modes)
- Flight Management Annunciators display

These buttons are highly specific and would probably not be usable in other aircrafts. However the very specific code used to produce these buttons is an example for alternative development.

# Toliss Airbus FCU

ToLiss FCU representation is actually a set of representations for Loupedeck LoupedeckLive side LCD display. As shown on the capture below, it vertically presents FCU data next to the encoders.

![[ToLiss-FCU-vert.png]]

There also is a horizontal version for the Stream Deck + horizontal display.

![[ToLiss-FCU-horiz.png]]

# Toliss Airbus FMA Display

The specific ToLiss FMA representation is made for long horizontal displays, like Stream Deck + (plus) LCD display.

![[ToLiss-FMA.png]]

> [!TIP] Page Switch
> It is easy to configure the LCD screen to change page between "FCU" and "FMA" allowing both screen to co-exist.

In the FMA page:

```yaml
  - index: touchscreen
    type: page
    page: fcu
```

and in the FCU page:

```yaml
  - index: touchscreen
    type: page
    page: fma
```

## Individual FMA

The FMA display can also be used to display a single FMA on a smaller square icon. 5 small square icons can be used to display all 5 FMAs.

![[ToLiss-FMA-single.png]]

It is also possible to display a single FMA (center), and loop through all FMAs by pressing on it.

Examples of such configuration are provided for ToLiss aircraft and can be used to develop alternate display on smaller decks.
