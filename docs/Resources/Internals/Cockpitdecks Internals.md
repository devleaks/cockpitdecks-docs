
Cockpitdecks proceeds by starting autonomous threads of execution that monitor different aspects of the interaction of decks with X-Plane.

Cockpitdecks threads are often created in start() procedures and terminated in terminate() procedures. Communication with the thread is performed through synchronous Queues.

In addition, each deck has its own, internal, mechanism to capture user interactions.

# Threads for Execution of Actions

As today, *things that occurs on a deck* are captured by a lower level computer software module. In the case of Cockpitdecks and its Streamdeck, Loupedeck and Berhinger devices, each of those lower level software module is designed to use a user-provided callback function that is called each time something occurs on the deck. That's how event enters Cockpitdecks.

During initialization of a deck, Cockpitdecks installs a small, minimalist callback function in the deck software module. This callback function processes data provided from the deck and immediately converts it into an [[Events|Event]] that gets enqueued right away for later handling. This process is optimized to be as minimal and as fast as possible. The Queue where events are then enqueued is called the **Event Queue**.

The Event Queue that receives all events from all decks connected to the system is unique for a Cockpit. It is the entry point for all interactions into Cockpitdecks. It behaves like a clean separator between lower level interaction handling at the device and Cockpitdecks. The callback function is responsible for decoding the information received from the device and crafting a typed Event that correspond to the interaction that occurred. The event also carries the necessary complementary information and data like, for instance, the precise button that was pressed or turned.

![[activations.svg]]

Inside Cockpitdecks' Cockpit, a thread of execution receives the event from the Event Queue and immediately executes them to perform the action they carry.

The action is executed in a separate thread of execution from those of the lower level physical interactions. Should execution of the action fail, the thread of execution of the capture is not affected.

# Thread for Dataref Monitoring

There is a similar mechanism for dataref values capture and processing.

In the Simulator entity, a thread monitors datarefs by collecting them as they arrive on UDP port. It compare each value with the last one captured and enqueue a "value changed" event into the **Dataref Update Queue** if it was updated.

Inside the simulator, another thread of execution receives the dataref value changed events and submit them for processing.

![[dataref updates.svg]]

Each dataref maintains a list of buttons that use/rely on it, and each of those buttons gets notified of the change to adjust. Each button receiWhen a button receives the message that one of its dataref has changed, it can adjust its internal state, and if it is currently rendered on a deck, adjust its display.
# Thread for Connection to X-Plane

In the Simulator, a thread permanently monitors the connection of Cockpitdecks to X-Plane. When there is no connection, the thread attempt to initiate a new connection until it succeeds. 

When a new connection is created, it immediately request dataref updates and update all decks with all dataref values.

If the connection breaks, it restarts its attempts to connect.

When there is no connection to X-Plane, Cockpitdecks works as expected, however, no command get issued to X-Plane, and no dataref value gets collected, hence, no deck icon gets updated to reflect the state changes.

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

[[Laminar X-Plane and Toliss Specific Buttons|More...]]