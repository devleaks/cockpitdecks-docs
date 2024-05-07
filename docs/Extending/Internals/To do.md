Isolate a framework that register and observe datarefs, make a better tool than NASA connector.

Evolve towards a XPPython UDP library.

When dataref(s) meet conditions, events are triggered.

Usable for observation, data collection, even « co-pilot » type of interaction: if this then that.

Make a350/a380 type tick marks on rotating knobs. LED around knob

Make a icon-builder app (electron?) very much like TouchPortal... So don't do it.

### Abstraction of command

Allow for « local » commands like change page, stop, inspect.

Command has a list of optional/mandatory arguments, especially local commands.

### Large displays

Gently pressing on a part of the display triggers FCU mode changes like Speed/Mach, Heading/Track, or even knobs push/pull (with long press). May be vertical swipes will increase/decrease values.

# Development Process

## Dreams

Add OSC/MIDI relay type to be able to create cockpit in OSCTouch.

Behave similarly as TouchPortal

## Status

Should be easy for software developer.

Installation not easy if not familiar with Python eco-system.

Need to hunt for datarefs and commands in X-Plane (using DataRefTool for example.)

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

Redo StringIcon so that it uses string udp fetches

May be make string fetch part of Simulator and not FMA() (i.e. more generic)

Get notified with string is updated through listener, etc.

Idea: Make Streamdeck+ touchscreen either 4 x (200x100) buttons with icon, or 8 x (100x100) icons

Suppress options: ?

Trigger push event on PRESS not on RELEASE (or add an option to force it)

This is because PRESS event have no release event.

redo string collector to collect dataref-string attributes, that can be on any button, e;g.aircraft, FMA, etc.
