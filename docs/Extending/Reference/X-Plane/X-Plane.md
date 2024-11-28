[X-Plane](https://en.wikipedia.org/wiki/X-Plane_(simulator)) is the name of a flight simulation software edited by [Laminar Research](https://www.x-plane.com).

For developers, X-Plane offers the following facilities:

- A Developer API which consists of library calls to integrate a development inside X-Plane.
- A network API, which outputs simulator data in UDP packets, and accepts on input some basic instructions to execute named commands, or change simulation internal variables and values.
- Another network API, a Web REST API to access internal variables and values.
- Most, if not all, configuration files are *documented* text files.

X-Plane maintains and exposes a large number of internal variables, called *datarefs*. X-Plane. For a running instance of X-Plane, there can be as much as 10,000 datarefs.

X-plane also produces a limited number of internal messages to notify of occurence of special events like another plane has been loaded by the user, orr the VR mode has been enabled. X-Plane messages are related to the X-Plane environment and program. There is no message related to the simulation itself. There is no message to tell a plane has taken off, or brought its landing gear down. There are only messages related to the program: New scenery was loaded, new plane was loaded, new sound bank was loaded, X-Plane has crashed (*!*), etc.

Cockpitdecks has three  methods to get variable values from X-Plane.

1. On startup, when a fairly static value needs to be fetched, the (newer) Web REST API is used to get the value of a dataref.
2. The regular method to get values of internal datarefs is a two-step process: First Cockpitdecks notifies X-Plane that it is interested in getting the value of a give dataref, and second, X-Plane sends the value at regular intervals and Cockpitdecks capture the value and interpret it.
3. String values that can change (annunciator displayed strings, path to resources, etc.) are collected by s special, dedicated X-Plane plugin that broadcast on the network (UDP) the values of these variables at regular time interval. Cockpitdecks captures and interpret the values of these string datarefs. This mechanism is limited to string-typed datarefs.
