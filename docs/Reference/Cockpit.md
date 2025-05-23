Cockpitdecks is an application that connect decks to the X-Plane flight simulator. On one side, the application scans for decks connected to the computer and prepare them for use with X-Plane. On the other side, Cockpitdecks connects to X-Plane through the network to issue instructions to the simulator and listen to changes to reflect those changes on the deck device if it allows it.

To determine what to display on decks, which commands to issue to the simulator, etc. Cockpitdecks reads a set of *configuration files* on startup.

![[intro-detailed.png|600]]

Configuration files are specific to an aircraft. Commands are different on a Cessna and on an Airbus. Things to display on the deck are different as well. That’s why configuration files are located in the folder of the aircraft being used because they are specific to it. Numerous other software like X-Camera proceed in a similar way, locating their aircraft specific configuration there as well.

The Cockpit is the *Maestro* component of the Cockpitdecks application.

It starts up the entire Cockpitdecks application. It establishes connection to the simulator, scans for existing decks connected to the computer, and load the aircraft configuration files. It listen to interactions that occur on the decks and listen to simulator changes to reflect deck statuses. It also monitors which aircraft is currently loaded in the simulator, and if the user changes aircraft, it loads the new configuration if any.

The following pages describe the necessary configuration files, their location and organisation, and their content. Configuration files are organized in structured folders, and this structure is explained as well.

All configuration files for Cockpitdecks are *Yaml-formatted files*. Yaml file contains a structured list of (name, value) pairs. The name is referred to as an *attribute*. The *value* can be almost anything: A number, a string, a list of things, or a list of other attributes.

# Cockpitdecks Configuration for one Aircraft

Decks are particular to one aircraft. All files necessary to Cockpitdecks are located in a single folder named `deckconfig` that is found in the folder of the X-Plane aircraft currently being used. In that folder, Cockpitdecks will find all its configuration files.
