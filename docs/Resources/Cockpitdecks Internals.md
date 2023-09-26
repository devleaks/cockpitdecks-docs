
Cockpitdecks proceeds by starting autonomous threads of execution that monitor different aspects of the interaction of decks with X-Plane.
In addition, each deck has its own, internal, mechanism to capture user interactions.

# Threads for Execution of Actions

Cockpitdecks threads are often created in start() procedures and terminated in terminate() procedures.

Most of the callback are executed directly from the caller. If a user presses a key, the device driver will detect it and execute a callback to excute the function. That callback directly execute the function which, often, consists of executing a command in X-Plane and refreshing the pressed key icon to reflect the new state.
Sometimes, callbacks are executed indirectly. The callback function enqueues a request for action. In this case, a separate monitoring thread dequeues the request for action and does it.

# Thread for Dataref Monitoring

A thread monitors datarefs by collecting them as they arrive on UDP port, compare each value with the previous one and fire a "value changed" event if detected.
Each dataref maintains a list of buttons that use/rely on it, and each of those buttons gets notified of the change to adjust. When a button receives the message that one of its dataref has changed, it can adjust its internal state, and if it is currently rendered on a deck, adjust its display.

# Thread for Connection to X-Plane

A thread permanently monitors the connection to X-Plane. When there is no connection, the thread attempt to initiate a new connection until it succeeds. When it created a new connection, it immediately request dataref updates and update all decks with all dataref values.
If the connection breaks, it restarts its attempts to connect.
When there is no connection to X-Plane, Cockpitdecks works as expected, however, no command get issued to X-Plane, and no dataref value gets collected, hence, no deck icon gets updated to reflect the state changes.

# (Internal) Utility Threads

When a Page Reload is requested, the requesting button may be deleted during the process. To prevent this, some operations like Page Reload or Cockpitdecks Stop are not executed directly. They are enqueued in an execution queue, and performed by another, external thread that will survive at least as long as the execution of the process requires it. (For Cockpitdecks Stop function, it is actually the thread that will terminate all other thread and then commit suicide.)

# Naming Conventions, etc.
## Default Values
When a value can be used by numerous variants and propose a default value, all "parents" object will propose the value as `default-...`. However, in the leaf object where the value is used, the `default-` part disappears from naming:
Deck or Page parent objects have `default-icon-color` attribute, while leaf Button that actually use the icon color has `icon-color` attribute.

# Drawn Buttons
It all started with simple annunciators! Most buttons are not static images but rather  drawn on the fly(!) from attributes in their definition (text, options, sizes…) and dataref values. This allow for extreme flexibility. The basic, simple image processing/drawing package used (Python Pillow) is the limit. It allows for stylish glow and lightly readable display when the button is in its off state (called *Korry* style).
Unstoppable after the design of annunciators, rotating switches and simpler push buttons where also created with drawings to replace static images, together with labels around them. It does not always appear like in the cockpit, but close enough and it does the work. But above all, it no longer is necessary to crop screen dump images and end up with a zillion icon images to reflect different states, or button positions, the price to pay is to adjust a few parameters in the definition of the button.

# Internal « Datarefs »
Datarefs whose name starts with `local:` behave like any other datarefs but are neither forwarded to X-Plane, nor read from it. They can be set, read, etc. like any other datarefs allowing for a kind of *inter-button communication*: one button sets it, another one adjust its appearance based on it, even buttons on another deck!

Truly, the sky is the limit. Enjoy.

# Dataref Collector

The DrefCollector is an Activation meant to be used for collecting large amount of slow changing datarefs like, typically, weather information.
Weather in X-Plane uses no less that 200 datarefs that could be fetched to provide information about the weather around the aircraft.
The DrefCollector splits the large collection of dataref in batches of a limited size and fetch those datarefs batch after batch. Ultimately, when all batches have been fetched, the entire collection is ready to be used.
The DrefCollector is monitored periodically for batches that do not get updated frequently.
DrefCollector exposes all datarefs it fetched in a `dref_collection` attribute.
# Weather Viewer

The WeatherViewer is a Representation that uses all weather-related datarefs to provide weather information to pilots.
(The Weather Viewer uses the DrefCollector activation.)