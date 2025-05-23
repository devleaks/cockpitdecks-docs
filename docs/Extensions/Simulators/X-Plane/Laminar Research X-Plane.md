[X-Plane](https://en.wikipedia.org/wiki/X-Plane_(simulator)) is the name of a flight simulation software edited by [Laminar Research](https://www.x-plane.com).

For developers, X-Plane offers the following facilities:

- A Developer API which consists of library calls to integrate a development inside X-Plane.
- A network API, which outputs simulator data in UDP packets, and accepts on input some basic instructions to execute named commands, or change simulation internal variables and values.
- Another network API, a Web REST API to access internal variables and values.
- Yet another netowkr API, based on WebSocket to access internal variables, issue commands, and get notified of variable changes and command activation.
- Most, if not all, configuration files are *documented* text files.

X-Plane maintains and exposes a large number of internal variables, called *datarefs*. X-Plane. For a running instance of X-Plane, there can be as much as 10,000 datarefs.

X-plane also produces a limited number of internal messages to notify of occurence of special events like another plane has been loaded by the user, orr the VR mode has been enabled. X-Plane messages are related to the X-Plane environment and program. There is no message related to the simulation itself. There is no message to tell a plane has taken off, or brought its landing gear down. There are only messages related to the program: New scenery was loaded, new plane was loaded, new sound bank was loaded, X-Plane has crashed (*!*), etc.

Cockpitdecks has two methods to get variable values from X-Plane.

1. On startup, when a fairly static value needs to be fetched, the (newer) Web REST API is used to get the value of a dataref.
2. The regular method to get values of internal datarefs is a two-step process: First Cockpitdecks notifies X-Plane that it is interested in getting the value of a give dataref, and second, X-Plane sends the value at regular intervals and Cockpitdecks capture the value and interpret it.

Finally, X-Plane externalize a list of commands that can be executed through any of the above API.

Since datarefs and commands are sometimes specific to an aircraft, it is advisable to reload and/or somehow reset all variable and command references when loading a new aircraft.
