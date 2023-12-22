X-Plane connector through UDP uses the following threads of execution to permanently collect dataref values from X-Plane as requested by deck displays.

# Connect Loop
The *connect loop* monitors the connection to X-Plane.
If there is no connnection, if it cannot connect to X-Plane to submit a command, or if it does not receive any data periodically, the *connect loop* attempts to connect until it succeeds.
When a new connection is established, the *dataref collection loop* is started and buttons get notified of dataref value changes.
The connect loop is created and managed by the `XPlaneBeacon` class.

# Dataref Value Collection Loop
The *dataref collection loop* permanently collects data from X-Plane and notifies buttons of dataref value updates.
If it cannot connect to X-Plane, or if the data collection otherwise fails, a time out occurs.
When several times out have occured, the collection process disconnects and terminates. (There is no need to monitor dataref values if we cannot collect them.) It will be restarted by the connect loop once a new connection is established.
The *dataref collection loop* is created and managed by the `XPlaneUDP` class.
