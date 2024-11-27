The Simulator entity represent the simulator software and contains all procedures necessary for interaction with the flight simulation software.

There currently is only one type of flight simulation software supported and it is Laminar Research X-Plane (release 12 onwards).

It is possible to create a new Simulator implementation for another software, like for example Microsoft Flight Simulator.

In addition to the core simulation software, the Simulator also proposes a few entities that represent interactions with it.

# Simulator Data Values

The simulation software presents to Cockpitdecks all its reachable « values ». Any value that is accessible in the simulation software is made available to Cockpitdecks through the SimulationData. A sophisticated mechanism updates the data in Cockpitdecks every time the data is modified in the simulator.

For example, Laminar X-Plane extensively uses « *datarefs* », while Microsoft Flight Simulator uses « *simvars* ». They both are values coming from the simulator, and they both can be used in Cockpitdecks.

## Local Values

In addition to simulator data, Cockpitdecks maintains its own set of « local » data, data that is not forwarded to the simulator but that can be used like any other simulator data.

# Instructions

Cockpitdecks offers the Instruction, an action that must be carried out in the simulator software, provided that the simulation software allows for « external action » to be accepted.

Again, as an example, Laminar X-Plane has « *commands* » (offered in two modes of operation), and Microsoft Flight Simulator has « *idontknow* ».

# Simulator Core

The Simulator entity is responsible for maintaining a connection to the Simulator software to collect simulator data values and issue instructions.
