As simple as it may appears when working, Cockpitdecks is a complex piece of software that relies on numerous technologies, systems, and interfaces to provide, ultimately, a confortable user experience.

First of all, Cockpitdecks tries to provide a uniform representation of different deck models. Each deck model, from different manufacturers, has its own way of doing things. Different decks are accessed differently, some through basic serial (USB) interfaces, some through application programming interfaces, and some other through existing "protocols" made to talk to devices like HID or MIDI. Some device even allow several methods to be used.

Cockpitdecks uses the appropriate method to hide the complexity of accessing the deck devices, to hide their particularities, at the expense of a complex and modular installation process. Some will use a single device, some other will use more than one, combining different models and brands to suit their needs.

Cockpitdecks communicates with X-Plane through the network UDP protocol. This offers the advantage that Cockpitdecks and X-Plane do not necessarily need to run on the same computer, as long as both computer are on the same local network.

Through UDP ports, X-Plane reports some internal parameter values (called datarefs), and accepts commands to execute.

Never ever forget that in the UDP protocol, there is no guarantee of delivery, ordering, or duplicate protection.
# Installation

Cockpitdecks is a regular python application and will run with python 3.10, or newer.
It is recommended to create a virtual environment and run Cockpitdecks within that environment. A set of packages need to be installed in that environment before Cockpitdecks can run.

1. Create a directory and download Cockpitdecks software from github.
4. Create a python environment.
2. Switch to that environment.
3. Install necessary python packages.
4. Test run Cockpitdecks without X-Plane.
5. Install optional Deck Helper plugin in X-Plane
6. Download and install aircraft deck configurations and layout.
7. Start Cockpitdeck application.
8. Start X-Plane.
9. Enjoy all your deck devices activated in X-Plane.

## Install Software

### Install Cockpitdecks Software

Create a new folder, in that folder:

```shell
$ git checkout https://github.com/devleaks/cockpitdecks.git
```

Create a new python environment and activate it.

### Install Python Packages

This packages are required by Cockpitdecks.

```shell
$ pip install ruamel.yaml pillow
```

Optionally, if you would like to use Weather or METAR buttons, add the following packages:

```
$ pip install avwx-engine scipy suntime timezonefinder
```

### Install Deck Devices Drivers

To have Cockpitdecks manage Streamdeck devices, add
```shell
$ pip install streamdeck
```

To have Cockpitdecks manage Loupedeck devices, add
```shell
$ pip install
git+https://github.com/devleaks/python-loupedeck-live.git
```

To have Cockpitdecks manage Touch Mini devices, add
```shell
$ pip install
git+https://github.com/devleaks/python-berhinger-xtouchmini.git
```

### Install Cockpitdecks Helper Plugin

Some commands cannot be executed directly through UDP. For exemples, commands that have a start and an end cannot be started or ended though UDP. It is an X-Plane UDP limitation.
To circumvent this, Cockpitdeck provides a small python plugin called the Cockpitdecks Helper plugin, that need to be installed into X-Plane to allow for start and end commands. The Cockpitdecks Helper plugin will execute start and end commands on behalf of the Cockpitdecks application. Cockpitdecks Helper plugin just need to be installed and will provide its services to Cockpitdecks. This plugin does not take any resource, it only adds and removes commands each time an aircraft is loaded.
The Cockpitdecks Helper Plugin works automatically, reads `deckconfig` configuration and creates a pair of (beginCommand, endCommand) for each *long press* command.
Cockpitdecks Helper Plugin is written in the python language. So it needs the [XPPython3](https://xppython3.readthedocs.io/) X-Plane plugin installed. XPPython3 plugin allow for execution of python code inside X-Plane.
The Cockpitdecks Helper plugin is not necessary if Cockpitdecks is installed as an X-Plane python plugin. (See below.)

Cockpitdecks XPPython3 plugins are located in the

```sh
cockpitdecks/resources/xppython3-plugins
```

folder. There are 2 single files.

To execute long press commands, the **Cockpitdecks Helper** plugin needs to be installed in XPPython3 PythonPlugins folder.

```shell
... /X-Plane 12/resources/plugins/PythonPlugins/PI_decks_helper.py
```

To fetch string-typed datarefs, the [[String Datarefs|String Dataref UDP Poster]] needs to be installed in XPPython3 PythonPlugins folder.

```shell
... /X-Plane 12/resources/plugins/PythonPlugins/PI_string_dataref_upd.py
```

# Usage

## Disconnect OEM Applications

First, you have to completely stop (quit completely) original manufacturer deck configuration applications. They take exclusive access to the device and that prevents Cockpitdecks from finding and using them.

## Start Cockpitdecks

### Test Start Cockpitdecks

If your decks are connected, and all drivers properly installed, you can test start Cockpidecks by simply launching the application without aircraft folder specified. Cockpitdecks will use default values for everything and set up each deck such as the first key can be used to toggle X-Plane map on or off.

```shell
$ python bin/cockpitdecks_udp_start.py
```


To start Cockpitdecks, use the `cockpitdecks_udp_start.py` script by supplying the X-Plane aircraft folder where deck confit resides. Start the python script and supply the folder name where `deckconfig` folder resides.

```shell
$ python cockpitdecks_udp_start.py "/Applications/X-Plane 12/Aircraft/Extra Aircraft/Toliss A321"
```

Cockpitdecks will look for `deckconfig` folder in the aircraft folder and start.

Cockpitdecks will repetitively try to connect to X-Plane. If it fails to connect, it infinitely tries again until it succeeds. If not connected, decks will load but no command will be issued to X-Plane and no data will come from X-Plane to update decks.
When Cocpiktdecks successfully connects to X-Plane, it refreshes all pages by *reloading* them to reflect the state of the aircraft on the decks.
If Cockpitdecks fails to connect to X-Plane or notices it does no longer receive dataref values from X-Plane, it will again repetitively try to connect to it until it succeeds.

The *aircraft folder* (Toliss A321) where cockpitdecks tries to find a `deckconfig` folder can be anywhere, it does not need to be in the X-Plane aircraft folder. However, the `deckconfig` folder must be in the X-Plane aircraft folder for the Cockpitdecks Helper Plugin. (For Unix technical people, a symbolic link does the trick.)

## Install Aircraft Config

Duane, a Cockpitdecks aficionado has realized several configurations for several aircrafts.
You can [find them here](https://github.com/dlicudi/cockpitdecks-configs), download them and install them in your aircraft folder.

# Troubleshooting

To report an issue with Cockpitdecks, you should always include the `XPPython3.log` file created in the X-Plane folder. Cockpitdecks also create a `cockpitdecks.log` files with more information.

The level of information produced in the file is controlled by the logging level parameter. (info=some information and warnings, debug=a lot of information for debugging purpose, your XPPython3.log file may grow quite large.) The parameter is available at the global plugin level (the entire plugin will report all messages), or can be set at a Cockpitdecks internal module level to pin point issues.
