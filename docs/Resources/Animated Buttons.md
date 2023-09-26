Typical buttons display an information in a static way, an icon or a LED that is updated when a status or a value has changed.
Animation is a button representation option that consists of continuously sending representation updates to continuously change the button's appearance.
For example, in its simplest form, a icon animation consist of sending a different images to a key at regular interval, cycling to a list of images (vey much like a GIF animation...). Another simple animation consists of making an icon «blink» on or off. A more complex animation can consist of a parametrized drawing, an image is drawn at each iteration and sent to a key.

Animations are automatic. They should not be confused with button appearance updates that occur automagically when underlying button values change.

Most of the time, the value button determine if the animation runs. When the value of the button is On, the animation runs. When Off, the animation does not run and may displays an alternate representation. 

# Icon-Based Animation
An icon-based animation is a procedure that changes the icon to display at regular internal.
The icon to display is specified through its index value in the list of icons available to the button (through the `multi-icons` attribute).

`icon-off`: When not running, an alternate icon can be displayed.

# Blinking Animation
A blinking annimation is an procedure that forces some of all part of a Representation alternatively On and Off and provoque a rendering update after changes, leading to a blinking button effect.

In this case, the animation controls and changes the value of the button, switching between On and Off states. The representation changes accordingly.

# Drawing Based Animation
A Drawing animation is a parametrised drawing that uses a single parameter value (for exemple the button's value). When the value changes, the procedure optionally uses a tweening algorithm to progressively change the value from the old one to the new one.

As a proff of this concept, the FollowTheGreens Representation displays a portion of taxiway centerline with "flashing" lead light when it is activated.

Animations do not need to be fast. As another proof of concept, the Weather representation pictures à Metar report on a icon. The representation is updated each time the Metar is updated (about every 30 minutes). It is another form or use of animation.