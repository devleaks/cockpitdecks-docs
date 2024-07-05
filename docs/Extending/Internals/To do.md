# General , Architecture

Isolate a framework that register and observe datarefs, make a better tool than NASA connector.

Evolve towards a XPPython UDP library.

When dataref(s) meet conditions, events are triggered.

Usable for observation, data collection, even « co-pilot » type of interaction: if this then that.

Make a icon-builder app (electron?) very much like TouchPortal... So don't do it.

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

Trigger push event on PRESS not on RELEASE (or add an option to force it) **DOCUMENT!**

This is because PRESS event have no release event.

Check aircraft changed event (based on acf_ICAO?)

# New Representations

Example ramp on side of LoupedeckLive or on touchscreen of sd+.

Tape like display (rose, speed, vertical, horizontal)

  First make tape, the simple translate+clip.

Simple gauges

Make a350/a380 type tick marks on rotating knobs. LED around knob

# Documentation

For activation/representation attributes, make table rather than subsections.

Same for activation statuses.

Add button/activation class attribute can be button status variables.
