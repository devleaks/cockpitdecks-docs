Cockpitdecks uses a limited set of rendez-vous variables with special purpose attached to it.

Rendez-vous variables are variables that are maintained by Cockpitdecks for convenience purpose. Other elements like buttons, observables, etc. can subscribe to these variables to be notified from situation changes.

# aircraft-name

The internal variable `aircraft-name`  is a string variable that expects the relative path to the currently used aircraft.

When the value of the variable changes, it is assumed a new aircraft is being loaded and Cockpitdecks undergo its change aircraft procedure.

# livery-name

Similarly, he `livery-name` variable expects the relative path to the current livery. If the livery changes, Cockpitdecks undergoes the livery change procedure.

# aircraft-icao

This variable contains the current aircraft ICAO code. It is used for information purpose. No Cockpitdecks behavior is affected by this variable. Several aircrafts May have the same code and it is not possible to determine which particular aircraft model/editor/publisher is being used.

Other information-only variables are also provided for convenience:

- airport-departure
- airport-arrival
- metar-departure
- taf-arrival
- metar-arrival
- flight-level
- parking-departure
- parking-arrival

All these variables are provided by Cockpitdecks in a Flight structure.

Variable values are set if available through the simulator variables, otherwise, they remain empty.

# Variables from Simulator

- date and time of simulation (local time simulated Zulu time)
- Simulation speed and replay status
- Location of aircraft

From these « raw » values, the following convenience variables are also provided (through observables):

- closest weather station ICAO code
- daytime/night time (based on simulation time, location, etc.)
