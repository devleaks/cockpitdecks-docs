# Start-Up and Initialisation

On startup, Cockpitdecks does its best at discovering what features are available to it.

What are the currently connected USB devices? Are there decks Cockpitdecks knows connected to the computer?

Is there a local flight simulator software? Which one? Where is it located? If a known simulator software is installed, does it have the necessary feature?

Did we supply information for a remote installation?

If Cockpitdecks X-Plane extension is installed, Cockpitdecks also listen to potential X-Plane beacons that would betray one or more running instances of X-Plane.

Cockpitdecks then loads its internal configuration including core fonts, images, observables, etc.

It then scans USB ports for available known physical decks.

```
start.py:<module>:505: Initializing Cockpitdecks..
cockpit.py:add_extensions:627: loaded extensions cockpitdecks_ld, cockpitdecks_xp, cockpitdecks_bx, cockpitdecks_sd, cockpitdecks_tl, cockpitdecks_wm, cockpitdecks_ext
cockpit.py:init:647: available simulators: NoSimulator, X-Plane
cockpit.py:init:650: available deck drivers: xtouchmini, loupedeck, streamdeck, virtualdeck
cockpit.py:init_simulator:688: simulator driver XPlane 2.8.0 installed
cockpit.py:init_simulator:690: COCKPITDECKS_PATH=/Users/xplane/X-Plane 12/Aircraft/Extra Aircraft:/Users/xplane/X-Plane 12/Aircraft/Laminar Research
cockpit.py:load_cd_fonts:1270: 24 fonts loaded, default font=D-DIN.otf, default label font=D-DIN.otf
cockpit.py:load_cd_icons:1212: 19 icons loaded from cache
cockpit.py:load_cd_icons:1242: default icon name inop.png found
cockpit.py:load_cd_sounds:1293: 9 sounds loaded
cockpit.py:load_cd_observables:1196: loaded 3 observables
cockpit.py:load_cd_deck_types:1186: loaded 8 deck types (LoupedeckLive, X-Touch Mini, Stream Deck Original, Stream Deck Mini, Stream Deck Neo, Stream Deck +, Stream Deck XL, Streamdeck)
cockpit.py:load_cd_deck_types:1187: loaded 11 virtual deck types (Virtual Deck for Development, virtual loupedeck.ct, virtual loupedeck.live.s, Virtual LoupedeckLive with Mosaic, Virtual LoupedeckLive, Virtual X-Touch Mini, Virtual Streamdeck Mini, Virtual Streamdeck MK.2, Virtual Stream Deck Neo, Virtual Streamdeck +, Virtual Streamdeck XL)
cockpit.py:load_cd_defaults:1385: added default system font Monaco.ttf
cockpit.py:scan_devices:770: device drivers installed for xtouchmini 1.3.6, loupedeck 1.4.6, streamdeck 0.9.6
cockpit.py:scan_devices:771: scanning for decks and initializing them (this may take a few seconds)..
cockpit.py:scan_devices:787: requirements xtouchmini>=1.3.6;loupedeck>=1.4.5;streamdeck>=0.9.5 satified
cockpit.py:scan_devices:803: found 0 xtouchmini
cockpit.py:scan_devices:803: found 1 loupedeck
cockpit.py:scan_devices:803: found 0 streamdeck
start.py:<module>:507: ..initialized

```

After the initialization of the Cockpit, Cockpitdecks loads either an aircraft configuration, if supplied, or a demonstration web deck.

After loading the aircraft, Cockpitdecks starts its internal monitoring processes, including the application server if requested to do so.

# Loading the Aircraft

When loading the aircraft, Cockpitdecks first inspect what is available and loads aircraft specific resources like fonts, images, observables, and virtual decks if any.

After, it loads and prepare each deck for this aircraft. Cockpitdecks initialise each deck by loading its Layout and all Pages it contains.

For each Page, Cockpitdecks loads all buttons on that page and prepare them for use.

# Loading a Button

For each button on a Page, Cockpitdecks goes through the same steps.

It first identifies what the button will do and check the button is capable of this action. This is done with the help of the definition of the button in the Deck Type (Button Type).

Cockpitdecks prepare the button activation and the Instructions the button will require to execute when *activated*. The button is now ready to interpret the events it receives from that button on the deck when it is manipulated.

Then Cockpitdecks identifies what the button will send as a feedback to the deck. It identifies the Representation of the button and check the button is capable of that representation.

The Representation is governed by the value of the button. Cockpitdecks therefore identifies the Variables that are necessary to compute the value of the button. It creates and initialise each variable if necessary. If an initial value is supplied, it is set and the button is ready to be *rendered* on the deck.

The button is now ready.

## Page

The Page merely behaves as a collection of buttons. A Page is the entity that is requested to be displayed on a deck. So when a deck request a Page to be displayed, the Page collects all information from its buttons, and ask each button to *render*. Also, when a button is manipulated, it is the activation defined in the page for that button that is called.

When a Page is loaded, among the thing it does is collection all variables from all individual buttons on that page. If some variable values are coming from the simulation software, the Page will request the simulation software to now monitor those variables and be notified when they change.

When a Page is replaced by another one, it first ask the simulation software to stop monitoring  variables it was using, before the new Page ask the simulator to monitor its own set of variables.

# Event Loop

The Event Loop is the core loop of Cockpitdecks. It receives Deck Events from each individual deck, and Simulator Events from the simulation software. Each event is then executed in sequence. Deck events lead to execution of instructions. Simulator Events lead to updates of variables and buttons values, which in turn, often lead to rendering of updated representations on decks.`

![[deck-simulator3.png|600]]

# Decks

Decks interface to Cockpitdecks through a *driver* software. The *deck driver* software has two responsibilities for Cockpitdecks.

First it needs to collect the information it receives from its manipulations. Which button was pressed, when, etc. That information is provided to Cockpitdecks into a Deck Event. Cockpitdecks will then interpret the Event, pass it to its Activation which will interpret it and issue the necessary Instructions.

Second, the deck driver has to translate the Representation requests into code that can be understood by the deck: Draw an icon on that key, make a sound, turn the LED on, etc.

# Simulator Software

The simulation software has several responsibilities.

The first one is to communicate the overall status of the simulation software: Is it running, which version, which aircraft is currently loaded, what is the simulated date and time, is the simulation software running at normal speed?, where is currently located the aircraft.

The second responsibility is to interface with Cockpitdecks, the *simulator driver*:

- Collect the used variable values from the simulation software and communicate them to Cockpitdecks,
- Execute the instructions issued by Cockpitdecks in the simulation software.

# Variable and Variable Listener

A Variable is a named entity that contains a value. However, it is a complex entity that performs internal tasks.

One of its internal work is to detect when the value of a variable changes. In this case, the Variable can notify all entities interested in the new value. These entities are called Variable Listeners.

This mechanism brings automatism inside Cockpitdecks. For example, a button can express its interest in being notified when a variable changes. The simulator software collects the value and pushes the values it receives in the variable. If the variable detects its value has changed, it notifies its listeners. When the listener receives the message that the value has changed it can act accordingly, adjusts its state and appearance. All this happens automatically after submitting the new value received from the simulation software.
