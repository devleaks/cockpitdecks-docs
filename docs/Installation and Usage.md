As simple as it may appears when working, Cockpitdecks is a complex piece of software that relies on numerous technologies, systems, and interfaces to provide, ultimately, a confortable user experience.

First of all, Cockpitdecks tries to provide a uniform representation of different deck models. Each deck model, from different manufacturers, has its own way of doing things. Different decks are accessed differently, some through basic serial (USB) interfaces, some through application programming interfaces, and some other through existing "protocols" made to talk to devices like HID or MIDI. Some device even allow several methods to be used.

Cockpitdecks uses the appropriate method to hide the complexity of accessing the deck devices, to hide their particularities, at the expense of a complex and modular installation process. Some will use a single device, some other will use more than one, combining different models and brands to suit their needs.

Cockpitdecks communicates with X-Plane through the network UDP protocol. This offers the advantage that Cockpitdecks and X-Plane do not necessarily need to run on the same computer, as long as both computer are on the same local network.

Through UDP ports, X-Plane reports some internal parameter values (called datarefs), and accepts commands to execute.

Never ever forget that in the UDP protocol, there is no guarantee of delivery, ordering, or duplicate protection. This is inherent to the UDP protocol. When something is sent, it is never guaranteed that it will be received or acknowledged.

# Installation

Cockpitdecks is a regular python application and will run with python 3.10, or newer.

It is recommended to create a dedicated environment and run Cockpitdecks within that environment. A set of packages need to be installed in that environment before Cockpitdecks can run.

## Enable X-Plane UDP

(See X-Plane UDP manual. Will provide information here later.)

## Install Cockpitdecks Application

### Install Cockpitdecks Software

Create a new python environment and activate it. In that environment, issue the pip install command:

```shell
$ pip install git+https://github.com/devleaks/cockpitdecks.git
```

### Install Additional Optional Python Packages

If you would like to use Weather or METAR buttons, add the following packages:

```
$ pip install avwx-engine scipy suntime timezonefinder metar
```

### Install Deck Devices Drivers

> [!NOTE] No Physical Deck?
> If you only use web decks and no physical deck, you can skip this step.

To have Cockpitdecks manage Streamdeck devices, add

```shell
$ pip install streamdeck
```

To have Cockpitdecks manage Loupedeck devices, add

```shell
$ pip install git+https://github.com/devleaks/python-loupedeck-live.git
```

To have Cockpitdecks manage Touch Mini devices, add

```shell
$ pip install git+https://github.com/devleaks/python-berhinger-xtouchmini.git
```

### Install Cockpitdecks Helper Plugin

(You can do this step later, but some functions will not work or be available inside Cockpitdecks.)

X-Plane UDP has some shortcomings that prevent some operations with decks.

To circumvent this, Cockpitdecks provides a small python plugin called the Cockpitdecks Helper plugin, that need to be installed into X-Plane. The Cockpitdecks Helper plugin will execute some instructions on behalf of the Cockpitdecks application. Cockpitdecks Helper plugin just need to be installed and will provide its services to Cockpitdecks.

If not installed, some of the commands inside Cockpitdecks will work properly.

#### Long command execution

Some commands cannot be executed directly through UDP. For exemples, commands that have a start and an end cannot be started or ended though UDP. It is an X-Plane UDP limitation.

To execute long press commands, the **Cockpitdecks Helper** plugin needs to be installed in XPPython3 PythonPlugins folder.

The Cockpitdecks Helper Plugin works automatically, reads `deckconfig` configuration and creates a pair of (beginCommand, endCommand) for each *long press* command.

#### String Datarefs

To fetch string-typed datarefs, the [[Complement Plugin - String Datarefs|String Dataref UDP Poster]] needs to be installed in XPPython3 PythonPlugins folder.

See [[String Datarefs]] for details about this plugin.

#### Cockpitdecks Helper Plugin Installation

Cockpitdecks Helper Plugin is written in the python language. So it needs the [XPPython3](https://xppython3.readthedocs.io/) X-Plane plugin installed. XPPython3 plugin allow for execution of python code inside X-Plane.

Cockpitdecks XPPython3 plugin is located in the

```sh
< Cockpitdecks-installed-code > /cockpitdecks/resources/xppython3-plugins
```

folder in the source code. There is a single file.

To install both services described above, copy the plugin file to:

```shell
... /X-Plane 12/resources/plugins/PythonPlugins/PI_cockpitdecks.py
```

and ask XPPython3 plugin to reload the scripts.

### Install Aircraft Specific `deckconfig` Folders

Duane, a Cockpitdecks aficionado has realized several configurations for several aircrafts.

You can [find them here](https://github.com/dlicudi/cockpitdecks-configs), download them and install them in your aircraft folder.

# Cockpitdecks Usage

## Disconnect OEM Applications

First, you have to completely stop (quit completely) original manufacturer deck configuration applications. They take exclusive access to the device and that prevents Cockpitdecks from finding and using them.

## Adjust config.py

Cockpitdecks uses a single configuration file to define a few elements that cannot easily be guessed.

| Variable            | Definiton                                                                                                                                                                    |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `XP_HOME`           | Home directory of X-Plane on the computer where Cockpitdecks runs. If X-Plane is installed on a remote host, `XP_HOME` must be `None`.                                       |
| `XP_HOST`           | Hostname or IP address where *X-Plane* runs.                                                                                                                                 |
| `APP_HOST`          | Tuple (Hostname or IP address, port) where *Cockpitdecks* application runs. If specified through operating system environment variables, use (**APP_HOST** and **APP_PORT**) |
| `API_PORT`          | X-Plane (12.1.1 and above) where REST API runs                                                                                                                               |
| `API_PATH`          | X-Plane API root path                                                                                                                                                        |
| `DEMO_HOME`         | Directory where demonstration `deckconfig` files are found.                                                                                                                  |
| `COCKPITDECKS_PATH` | `COCKPITDECKS_PATH` is a colon-separated list of folder paths. Cockpitdecks will search for aircrafts in directories specified in this variable. First match is returned.    |

Most of these variables gets their default value from operating system environment variables. If no environment variable is defined, the `config.py` file supplies a value.

## Start Cockpitdecks

```
$ cockpitdecks-cli --help
usage: cockpitdecks-cli [ { --help | --demo | directory [fixed] } ]

--help, -h, -?: displays command line help and exits
--demo        : starts demo mode, X-plane not needed but used if available
directory     : start from directory if directory contains deckconfig directory
              : if 'fixed' added after the directory, aircraft will not change

without any argument:
  * if X-Plane runs on same computer as Cockpitdecks, starts in full automatic
    mode, reloading aircrafts when changes are detected.
  * if X-Plane does not run on same computer as Cockpitdecks, start demo mode.
```

There are two main configuration mode to start Cockpitdecks. Either Cockpitdecks runs on the same computer as X-Plane, or Cockpitdecks runs on a remote computer.

### Test Cockpitdecks In Demonstration Mode

```sh
$ cockpitdecks-cli --demo
```

In this mode, Cockpitdecks will start a single web deck.

### Cockpitdecks and X-Plane Run On Same Computer

In this case, make sure the configuration file is setup and issue

```sh
$ cockpitdecks-cli
```

Cockpitdecks will immediately start in demonstration mode and listen for X-Plane interaction. If Cockpitdecks finds that an aircraft is loaded and that Cockpitdecks `deckconfig` folder exists in the folder of that aircraft, Cockpidecks will load this configuration.

If no configuration is found, Cockpitdecks will listen and interpret X-Plane data but will not load a new configuration.

This mode is fully automatic, Cockpitdecks always attempts to load the current aircraft deckconfig configuration, if present. Similarly, the XPPython3 plugin, if installed, will load the new configuration as well.


> [!NOTE] COCKPITDECKS_PATH
> The COCKPITDECKS_PATH environment variable is not used when X-Plane and Cockpitdecks run on the same computer. In this case, Cockpitdecks will look into X-Plane Aircraft folder.

### Cockpidecks and X-Plane Run on Different Computer

In this case, it is not possible for Cockpitdecks to locate aircraft configuration files. A set of configuration file will need to be supplied to Cockpitdecks on the command line, and Cockpitdecks will not load a new configuration when it detects an aircraft change.

```sh
$ cockpitdecks-cli aircrafts/A21N fixed
```

The `fixed` argument ensure that Cockpitdecks will not reload a new aircraft if it detects the aircraft has changed.

If `fixed` is not present, Cockpitdecks will attempt to look for the new aircraft in all directories specified in the `COCKPITDECKS_PATH`. The aircraft must have a Cockpitdecks configuration folder `deckconfig`. If it finds such an aircraft, it will load the new configuration.

Cockpitdecks will look for `deckconfig` folder in the aircraft folder and start.

Cockpitdecks will repetitively try to connect to X-Plane. If it fails to connect, it infinitely tries again until it succeeds. If not connected, decks will load but no command will be issued to X-Plane and no data will come from X-Plane to update decks.

When Cockpitdecks successfully connects to X-Plane, it refreshes all pages by *reloading* them to reflect the state of the aircraft on the decks.

If Cockpitdecks fails to connect to X-Plane or notices it does no longer receive dataref values from X-Plane, it will again repetitively try to connect to it until it succeeds.

The *aircraft folder* (Toliss A321) where cockpitdecks tries to find a `deckconfig` folder can be anywhere, it does not need to be in the X-Plane aircraft folder. However, the `deckconfig` folder must be in the X-Plane aircraft folder for the Cockpitdecks Helper Plugin. (For Unix technical people, a symbolic link does the trick.)

#### For Cockpitdecks Developers

If you cloned Cockpitdecks software in a folder, you can start Cockpitdecks with

```shell
$ python start.py
```

from the top folder of the source code.

# Troubleshooting

To report an issue with Cockpitdecks, you should always include the `XPPython3.log` file created in the X-Plane folder. Cockpitdecks also create a `cockpitdecks.log` files with more information in the directory you started your script from.

The level of information produced in the file is controlled by the logging level parameter. (info=some information and warnings, debug=a lot of information for debugging purpose, your XPPython3.log file may grow quite large.) The parameter is available at the global plugin level (the entire plugin will report all messages), or can be set at a Cockpitdecks internal module level to pin point issues.

# Termination

Cockpitdecks is designed to terminates cleanly. All requested datarefs monitoring are cancelled, connections are closed, all threads are terminated and joined cleanly. However, it may sometimes take a few seconds before a thread terminates. For example, if a thread is meant to run every 30 seconds, it may be necessary to wait a full 30 seconds before the thread notices the termination request and quits. Longer threads (above 30 seconds or a minute) check periodically for termination request. Cockpitdecks should always terminate cleanly.

If necessary, pressing ++ctrl+c++ several time in the main window will stop Cockpitdecks completely right away.
