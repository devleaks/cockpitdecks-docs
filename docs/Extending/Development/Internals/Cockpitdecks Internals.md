As simple as it may appears when working, Cockpitdecks is a complex piece of software that relies on numerous technologies, systems, and interfaces to provide, ultimately, a confortable user experience.

First of all, Cockpitdecks tries to provide a uniform representation of different deck models. Each deck model, from different manufacturers, has its own way of doing things. Different decks are accessed differently, some through basic serial (USB) interfaces, some through application programming interfaces, and some other through existing "protocols" made to talk to devices like HID or MIDI. Some device even allow several methods to be used.

Cockpitdecks uses the appropriate method to hide the complexity of accessing the deck devices, to hide their particularities, at the expense of a complex and modular installation process. Some will use a single device, some other will use more than one, combining different models and brands to suit their needs.

Cockpitdecks communicates with X-Plane through the network UDP protocol. This offers the advantage that Cockpitdecks and X-Plane do not necessarily need to run on the same computer, as long as both computer are on the same local network.

Through UDP ports, X-Plane reports some internal parameter values (called datarefs), and accepts commands to execute.

Never ever forget that in the UDP protocol, there is no guarantee of delivery, ordering, or duplicate protection. This is inherent to the UDP protocol. When something is sent, it is never guaranteed that it will be received or acknowledged.

# Architecture

## Cockpitdecks Software Entities

![[entities.svg|600]]

Cockpitdecks proceeds by starting autonomous threads of execution that monitor different aspects of the interaction of decks with X-Plane.

Cockpitdecks threads are often created in start() procedures and terminated in terminate() procedures. Communication with the thread is performed through synchronous Queues.

In addition, each deck has its own, internal, mechanism to capture user interactions.

## Execution of Actions

As today, *things that occurs on a deck* are captured by a lower level computer software module. In the case of Cockpitdecks and its Streamdeck, Loupedeck and Berhinger devices, each of those lower level software module is designed to use a user-provided callback function that is called each time something occurs on the deck. That's how event enters Cockpitdecks.

During initialization of a deck, Cockpitdecks installs a small, minimalist callback function in the deck software module. This callback function processes data provided from the deck and immediately converts it into an [[Events|Event]] (a *Deck Event*) that gets enqueued right away for later handling. This process is optimized to be as minimal and as fast as possible. The Queue where events are then enqueued is called the **Event Queue**.

The Event Queue that receives all events from all decks connected to the system is unique for a Cockpit. It is the entry point for all interactions into Cockpitdecks. It behaves like a clean separator between lower level interaction handling at the device and Cockpitdecks. The callback function is responsible for decoding the information received from the device and crafting a typed Event that correspond to the interaction that occurred. The event also carries the necessary complementary information and data like, for instance, the precise button that was pressed or turned.

![[activations.svg|600]]

Inside Cockpitdecks' Cockpit, a thread of execution receives the event from the Event Queue and immediately executes them to perform the action they carry.

The action is executed in a separate thread of execution from those of the lower level physical interactions. Should execution of the action fail, the thread of execution of the capture is not affected.

## Dataref Monitoring

There is a similar mechanism for dataref values capture and processing.

In the Simulator entity, a thread monitors datarefs by collecting them as they arrive on UDP port. It compare each value with the last one captured and enqueue a "value changed" event into the **Event Queue** if it was updated.

![[dataref updates.svg|500]]

Each dataref maintains a list of buttons that use/rely on it, and each of those buttons gets notified of the change to adjust. When a button receives the message that one of its dataref has changed, it can adjust its internal state, and if it is currently rendered on a deck, adjust its display.

# Thread for Connection to X-Plane

In the Simulator, a thread permanently monitors the connection of Cockpitdecks to X-Plane. When there is no connection, the thread attempt to initiate a new connection until it succeeds.

When a new connection is created, it immediately request dataref updates and update all decks with all dataref values.

If the connection breaks, it restarts its attempts to connect.

When there is no connection to X-Plane, Cockpitdecks works as expected, however, no command get issued to X-Plane, and no dataref value gets collected, hence, no deck icon gets updated to reflect the state changes.

# Internal Datarefs

Datarefs whose name starts with a prefix (currently `data:`) behave like any other datarefs but are neither forwarded to X-Plane, nor read from it. They can be set, read, etc. like any other datarefs allowing for a kind of *inter-button communication*: one button sets it, another one adjust its appearance based on it, even buttons on another deck!

Truly, the sky is the limit. Enjoy.

# Internal Resource Folder

In addition to aircraft specific definitions, Cockpitdecks contains in its core, a default configuration used as a fall back if no value is found at the aircraft specific level. These global core configuration is found in a `resources` folder inside Cockpitdecks software package. This folder should never be changed since it affects the entire Cockpitdecks application. It contains the following files and subfolders:

## config.yaml

This is a global level configuration file. It always is loaded first and can be overwritten by aircraft, deck, or page-specific variants.

## icons

Icons in this folder are available to all aircrafts.

## fonts

Fonts in this folder are available to all aircrafts.

Cockpitdecks provides a few fonts found here and there together with their respective copyright files.

## docs

A copy of Cockpitdecks documentation is included there. The documentation folder produced in the GitHub wiki of Cockpitdecks.

## Image files

The resource folder contains a few image files used as logos and wallpapers.

There is also an image with color names that can be used in `color` attributes.

## iconfonts.py

This file defines icon fonts. Icon fonts are fonts that are used to display iconic characters often named intuitively. Cockpitdecks comes with a copy of Font Awesome icons, and Weather Icons.

## constants.py

Defines a few constants that should never be changed. Change at your own risk.
