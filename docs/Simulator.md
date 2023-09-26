The Simulator represent the simulator software and contains all procedures necessary for interaction with the Flight Simulation software.

There currently is only one type of flight simulation software supported and it is Laminar Research X-Plane (release 12 onwards).
(It is possible to create a new Simulator implementation for another software.)

# X-Plane

X-Plane contains all interactions with X-Plane software.

- Initialization of connection, closing, and monitoring of the connection.
- Monitoring of datarefs currently in use
- Issuing of commands

A few parameters are necessary in  case Cockpitdecks cannot find X-Plane UDP Beacon on the local area network.

## Connection to X-Plane

Cockpitdecks permanently monitor the connection to X-Plane. If X-Plane is not started or not reachable, Cockpitdecks will regularly report the issue and continue trying to establish a connection to X-Plane.

When a new connection is established, Cockpitdecks will ask the simulator to start sending necessary datarefs values and will start dataref change monitoring. (See below.)

Cockpitdecks will continue monitoring the connection to X-Plane. If the connection is lost, for example if X-Plane is stopped, Cockpitdecks will notify all decks and Cockpitdecks will restart trying to establish a new connection until it succeeds. If there is no connection to X-Plane, there is no change of value of dataref that depends on X-Plane.

## Dataref Monitoring

Once a connection to X-Plane is established, Cockpitdecks will start monitoring required datarefs. Each time a dataref value has changed, all buttons relying on the value of that dataref will be notified of the change to, for exemple, adjust their appearance accordingly.

> There is a maximum value of datarefs that can be requested from the simulator (50).

### Dataref Sets Collector

See [[Button Value#Dataref Sets|Dataref Sets]].
## Commands

When a button is turned or pressed, it often is necessary to issue a command to X-Plane. If a connection to X-Plane is established, Cockpitdecks will ask X-Plane to execute that command.
If there is no connection to X-Plane, Cockpitdecks will report the command that should have been executed.