The Simulator entity represent the simulator software and contains all procedures necessary for interaction with the Flight Simulation software.

There currently is only one type of flight simulation software supported and it is Laminar Research X-Plane (release 12 onwards).

It is possible to create a new Simulator implementation for another software.

In addition to the core simulation software, the Simulator also proposes a few accessory classes that represent interactions with it.

# Simulator Data Values

The simulation software presents to Cockpitdecks all its reachable « values ». Any value that is accessible in the simulation software is made available to Cockpitdecks through the SimulationData. A sophisticated mechanism updates the data in Cockpitdecks every time the data is modified in the simulator.

# Instructions

Cockpitdecks offers the Instruction, an action that must be carried out in the simulator software, provided that the simulation software allows for « external action » to be accepted.

# Simulator Core

The Simulator entity is responsible for maintaining a connection to the Simulator software to collect simulator data values and issue instructions.
