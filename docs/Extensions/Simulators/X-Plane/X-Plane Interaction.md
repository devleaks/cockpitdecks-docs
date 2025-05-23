The interaction with X-Plane is two folds.

- Execution of commands through *Activations*.
- Monitor simulator parameter values and enqueue value updates.

But let's first explain how Cockpitdecks connects to X-Plane.

# X-Plane Connector

X-Plane connector through WebSocket uses the following threads of execution to permanently collect dataref values from X-Plane as requested by deck displays.

## Connect Loop

The *connect loop* monitors the connection to X-Plane.

If there is no connnection, if it cannot connect to X-Plane to submit a command, or if it does not receive any data periodically, the *connect loop* attempts to connect until it succeeds.

When a new connection is established, the *websocket message receiver loop* is started and enqueues events like value changes and simulator events.

# Activations

When a button is activated, Cockpitdecks will either submit a command, or ask to change the value of a writable dataref, or both.

# Dataref Value Monitoring

Dataref value monitoring occurs in two steps.

First, when a page is loaded in a deck, all dataref it uses and all events it is interested in are registered with the simulator for update.

The *websocket message receiver loop* permanently collects data from X-Plane and generate events for dataref value updates, or when a simulator event of interest occurs.

If the loop cannot connect to X-Plane, or if the data collection otherwise fails, a time out occurs.

When several times out have occurred, the collection process disconnects and terminates. (There is no need to monitor dataref values if we cannot collect them.) It will be restarted by the connect loop once a new connection is established.

The dataref collection loop immediately compare a dataref value with its previous value and enqueues a dataref change event.
