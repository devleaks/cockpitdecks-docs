Isolate a framework that register and observe datarefs, make a better tool than NASA connector.

Evolve towards a XPPython UDP library.

When dataref(s) meet conditions, events are triggered.

Usable for observation, data collection, even « co-pilot » type of interaction: if this then that.

Make a350/a380 type tick marks on rotating knobs. LED around knob

Make a icon-builder app (electron?) very much like TouchPortal... So don't do it.

### Large displays

Gently pressing on a part of the display triggers FCU mode changes like Speed/Mach, Heading/Track, or even knobs push/pull (with long press). May be vertical swipes will increase/decrease values.

# Dashboard

Create a page with dashboard-like icons:

To, From

Dist from takeoff, ATD

Dist to landing, ETA

Metar at departure, TAF at Arrival

Fuel, 1 per tank

Tire, landing gears

Elec volt

Hyd Pressure

AirCo "flow"

Oxy?

The sky is the limit...

# Refactor

(suppress Collector? that does not work very well?)

Idea: Make Streamdeck+ touchscreen either 4 x (200x100) buttons with icon, or 8 x (100x100) icons

Suppress options: ?

Trigger push event on PRESS not on RELEASE (or add an option to force it)

This is because PRESS event have no release event.

Check aircraft changed event (based on acf_ICAO?)

# New Representations

Example ramp on side of LoupedeckLive or on touchscreen of sd+.


# Redo, refactor

On startup, load & cache icons as found

DO NOT cache icon resized for deck


Make clever resize based on icon defs in deck_type, resize icon on the fly.
I.e. redo a generic pil_helper and get rid of deck-specific pil_helpers.
