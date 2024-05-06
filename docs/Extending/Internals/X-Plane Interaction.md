The interaction with X-Plane is two folds.

- Execution of commands through *Activations*.
- Monitor simulator parameter values and enqueue value updates.

But let's first explain how Cockpitdecks connects to X-Plane.

# X-Plane UDP Connector

X-Plane connector through UDP uses the following threads of execution to permanently collect dataref values from X-Plane as requested by deck displays.
## Connect Loop

The *connect loop* monitors the connection to X-Plane.
If there is no connnection, if it cannot connect to X-Plane to submit a command, or if it does not receive any data periodically, the *connect loop* attempts to connect until it succeeds.
When a new connection is established, the *dataref collection loop* is started and buttons get notified of dataref value changes.
The connect loop is created and managed by the `XPlaneBeacon` class.

# Activations

When a button is activated, Cockpitdecks will either submit a command, or ask to change the value of a writable dataref, or both.

It will also, optionally, execute another `view` command. The purpose of the view command is to modify the focus proposed on the screen consecutively to the first command executed. For example, when the `Status` ECAM button is pressed, the `view` command changes to show the ECAM display.


# Dataref Value Monitoring

Dataref value monitoring occurs in two steps.

First, when a page is loaded in a deck, all dataref it uses are registered with the Dataref Monitor.

Inside the dataref monitor, the *dataref collection loop* permanently collects data from X-Plane and notifies buttons of dataref value updates.

If it cannot connect to X-Plane, or if the data collection otherwise fails, a time out occurs.
When several times out have occurred, the collection process disconnects and terminates. (There is no need to monitor dataref values if we cannot collect them.) It will be restarted by the connect loop once a new connection is established.

The *dataref collection loop* is created and managed by the `XPlaneUDP` class.

The dataref collection loop immediately compare a dataref value with its previous value and enqueues a dataref change event into the *dataref change queue*.

# About Performances

## Volume

Decks sometimes have up to 30 buttons, and simulator pilots sometimes have several decks. During developments, Cockpitdecks controlled up to 6 decks. This led to the monitoring of an average of 80 different datarefs to change the appearance of 80 buttons or LED encoders.

We only request dataref values to be sent at the lowest emission rate, that is once per second.

A UDP packet of values contains at most 14 or 15 values. It takes a few packets to get all dataref values.

We experimentally observed that there is a performance limit of about ~100 datarefs that can be requested from X-Plane. After that, UDP packets will be emitted, but with some noticeable delay (up to a few seconds). And since they will be emitted late, their value will no longer reflect the current X-Plane value.

To request more datarefs, you can request them by batch of less than 100 datarefs and switch batches of requested datarefs at regular interval. It works very well.

Cockpitdecks created a [[Dataref Sets|dataref batch collector]].
