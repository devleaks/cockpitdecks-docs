Cockpitdecks provides a few, drawing-based, icons and animations to complement your cockpit or dashboard.

The following icons or animations provide each a limited set of customization attributes. Please recall that Cockpitdecks is not a drawing tool, but rather a template tool with custom icons ready to be used with a few customization attributes.

In each case, we will detail its intended use and the limits of the icon provided.

Also, these special icons exhibit particular design techniques that can be extended by Cockpitdecks Representation developers.

> [!WARNING] Work In Progress
> These icons are currently basic but do provide their function.
> They have limited customization, but these can be added over time later if necessary or requested.

# Display Tape

A Tape is a type of display that appears like a tape being slid under the screen. Here are famous display tapes on an Airbus Primary Flight Display:

- Speed tape on the left,
- Altitude tape on the right,
- Vertical speed gauge indicator on the far right,
- Heading tape at the bottom.

![[PFD.jpg]]

Tapes are often fitted With colorful marks that represent critical values for the indication being displayed.

Display tapes should not be confused with duct tapes, often used to fix aircrafts.

![[duct-tape.jpg|400]]

Duct tapes have one configuration attribute: `duct-tape-color`with default value `silver-grey`.

# Gauge

Cockpitdecks provides a small set of customizable gauge. Since gauges are rendered on small, often square, LCD screens, they are very limited in detail.

One gauge displays one value, and it updated when the value change.

A special gauge is the heading gauge (called `rose`) which show the heading on a compass. Compass can be adjusted North, on autopilot value, or on actual aircraft value (compass or true).

# Charts

Cockpitdecks provides a limited set of time-based [[Drawn Buttons – Charts|display charts]], also often called [sparklines](https://en.wikipedia.org/wiki/Sparkline).
