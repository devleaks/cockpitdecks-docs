A Mosaic is a trick used to split a large button into smaller ones.

A Mosaic can, in some regards, be considered as a special case of a Page. Yes, a small page with its own set of individual buttons.

Very much like a Page, a Mosaic contains buttons, with their own behavior (activation) and rendering (representation). However, at the end, all button rendering are sticked together, like *tiles* on a mosaic, to render on the larger button (the mosaic).

Typical example of Mosaic are: LoupedeckLive side buttons: They are larger, 60x270 pixels, and can be "split" into smaller buttons, like a column of 4 60x60 buttons, with 10 pixels between each. A Streamdeck Plus touchscreen, which is 100x800 pixel in size, can either be split into 8 buttons of 100x100 each, or less buttons with more space between them.

![[mosaic-ll.png|400]]

![[mosaic-sdplus.png|400]]

![[mosaic.png]]

Mosaic representation has been designed so that it requires the minimal adjustment to Cockpitdecks. Here are some design constraints.

# Activation

When *touching* the *Mosaic*, we need to know where the contact occurred because we have to determine which *tile* was activated. Therefore, the only valid activation for a Mosaic is [[Events#SwipeEvent|swipe event]], because the swipe event reports the coordinates (x, y) where the contact occurred.

Similarly, for *tiles*, for simplicity, we will only allow push and long-push events. We may, in future releases, allow for more events but cases will be requested before we make Mosaic too complicated.

In a nutshell, the swipe activation of the *Mosaic* gets transformed into a push, or long-push representation of a *tile*.

# Representation

The *Mosaic* representation will be an *image*. As its name says, it will be composed of smaller *tiles*, that are also *images*.

Therefore the representation of a button inside a Mosaic, a tile, must produce an image, like icon, all drawing representations (that indirectly create images), etc.

In a nutshell, the representation of the *Mosaic* is built from the representations of all *tiles* that are put together at their proper offset position.

# Deck Type Definition

Here is an example of definition of a mosaic for the LoupedeckLive left screen.

```yaml hl_lines="3 20"
  - name: left
    action:
      - swipe
    feedback: image
    dimension:
      - 60
      - 270
    layout:
      offset:
        - 120
        - 59
    options: corner_radius=4
    # The left button is composed of 3 sub-buttons:
    mosaic:
      - name: 0
        # prefix of mosaic buttons should match name of parent mosaic
        # names will be left0, left1, left2
        prefix: left
        action:
          - push
        feedback: image
        dimension:
          - 60
          - 60
        repeat: [1, 3]
        layout:
          offset:
            - 0
            - 15
          spacing:
            - 0
            - 30
```

# Button Definition

## Mosaic

``` hl_lines="2 4"
  - index: left
    type: mosaic
    label: MosaiK
    mosaic:
      tiles:
        - index: m0
          (...)
```

The activation (`type`) must be `mosaic`. This activation transforms the swipe event into push events.

The representation must be `mosaic`. THe representation collects the representation of each tile and compose the final image that is sent for display.

## Tiles

``` hl_lines="5"
  - index: left
    type: mosaic
    label: MosaiK
    mosaic:
      tiles:
        - index: m0
          name: RELOAD
          type: reload
          icon: RELOAD
          label: RELOAD
          label-position: ct
(more clever and realist button later, this is a working placeholder)
```

## Representation

(picture, later)
