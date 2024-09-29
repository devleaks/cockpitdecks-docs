![[installer.jpg|600]]

# Installation

Cockpitdecks is a regular python application and will run with python 3.10, or newer.

It is recommended to create a dedicated environment and run Cockpitdecks within that environment.

## X-Plane *!*

Cockpitdecks is a modern software that takes benefit from all available possibilities. While being developed, it is constantly updated to use the latest packages and features.

Therefore, Cockpitdecks will work better with the latest production release of X-Plane. It may not work properly, or some feature may not be available when using older versions of X-Plane. We do not have the engineering resources to maintain working versions of Cockpitdecks for all versions of X-Plane. While most features should still work in X-Plane 11, they were never tested, and we will not provide any support to make it work on older release.

Same occurs with aircrafts. Aircrafts are pieces of software that are regularly updated. It is a good practice to include the X-Plane and aircraft version information as comments in the `deckconfig` files, to precise which version of X-Plane or an aircraft is required to run properly.

As a practical example, X-Plane recently opened access to internal data through a new channel: [A Web REST API access](https://developer.x-plane.com/article/x-plane-web-api/). This is offered in X-Plane release 12.1.1 or newer. Immediately, Cockpitdecks has taken benefit from this new, simplified mean to access internal values.

It is good practice to maintain the software you use to the latest, production version. Cockpitdecks is no exception to this advise.

In particular, the X-Plane Cockpitdecks Helper plugin uses and requires the latest version of [XPPython3](https://xppython3.readthedocs.io/en/latest/index.html) plugin to work. This plugin itself requires X-Plane 12 and recent version of X-Plane SDK. Cockpitdecks Helper plugin is strictly not required to run Cockpitdecks but some features will not work without it.

### Enable X-Plane UDP

> [!NOTE] X-Plane 12 and UDP
> X-Plane 12 appears to disable UDP networking initially. Check Settings -> Network page, and make sure “Accept incoming connections” is enabled.
 
(See X-Plane UDP manual. Will provide information here later.)

## Install Cockpitdecks Application

### Install Cockpitdecks Software

Create a new python environment and activate it. In that environment, issue the pip install command:

```sh
pip install 'cockpitdecks[weather,streamdeck] @ git+https://github.com/devleaks/cockpitdecks.git'
```

Valid installable extras (between the `[` `]`, comma separated, no space) are:

| Extra         | Content                                                                                                                     |
| ------------- | --------------------------------------------------------------------------------------------------------------------------- |
| `weather`     | To load special iconic representation for weather. These icons sometimes fetch information outside of X-Plane. Recommended. |
| `streamdeck`  | For Elgato Stream Deck decks                                                                                                |
| `loupedeck`   | For Loupedeck LoupedeckLive, LoupedeckLive.s and Loupedeck CT devices                                                       |
| `xtouchmini`  | For Berhinger X-Touch Mini device                                                                                           |
| `development` | For developer only, add testing packages and python types                                                                   |

### Install Cockpitdecks Helper Plugin

> [!WARNING] Cockpitdecks X-Plane Helper Plugin
> You can do this step later, but some functions will not work or be available inside Cockpitdecks.

X-Plane UDP has some shortcomings that prevent some operations with decks.

To circumvent this, Cockpitdecks provides a small python plugin called the Cockpitdecks Helper plugin, that need to be installed into X-Plane. The Cockpitdecks Helper plugin will execute some instructions on behalf of the Cockpitdecks application. Cockpitdecks Helper plugin just need to be installed and will provide its services to Cockpitdecks.

If not installed, some of the commands inside Cockpitdecks will work properly.

#### Long command execution

Some commands cannot be executed directly through UDP. For exemples, commands that have a start and an end cannot be started or ended though UDP. It is an X-Plane UDP limitation.

To execute long press commands, the **Cockpitdecks Helper** plugin needs to be installed in XPPython3 PythonPlugins folder.

#### String Datarefs

X-Plane UDP only allows to fetch dataref values one by one. Retrieving a string is a tedious process.

To collect string-typed datarefs, the **Cockpitdecks Helper** plugin needs to be installed in XPPython3 PythonPlugins folder.

See [[String Datarefs]] for details about this.

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

Cockpitdecks `deckconfig` folder must be placed in the folder of the X-Plane aircraft to be found by Cockpitdecks or the Cockpitdecks Helper plugin.

```
<X-Plane 12 Folder>/Aircraft/Extra Airicraft/Toliss A321/deckconfig
```

# Cockpitdecks Usage

## Disconnect OEM Applications

First, you have to completely stop (quit completely) original manufacturer deck configuration applications. They take exclusive access to the device and that prevents Cockpitdecks from finding and using them.

## Adjust `config.yaml`

Cockpitdecks uses a single configuration file to define a few elements that cannot easily be guessed. Cockpitdecks provides a configuration file that is suitable for single computer installation. You must, however, adjust at least the X-Plane folder path.

| Variable   | Definiton                                                                                                                                                                                            |
| ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `XP_HOME`  | Home directory of X-Plane on the computer where Cockpitdecks runs. If X-Plane is installed on a remote host, `XP_HOME` must be `None`.                                                               |
| `XP_HOST`  | Hostname or IP address where *X-Plane* runs. Defaults to local host.                                                                                                                                 |
| `APP_HOST` | Tuple (Hostname or IP address, port) where *Cockpitdecks* application runs. If specified through operating system environment variables, use (**APP_HOST** and **APP_PORT**). Default to local host. |
| `API_PORT` | X-Plane (12.1.1 and above) where REST API runs. Default to 8086.                                                                                                                                     |
| `API_PATH` | X-Plane API root path. Currently default to `/api/v1`.                                                                                                                                               |

In addition, there is a operation system environment variable `COCKPITDECKS_PATH` that holds folders where Cockpitdecks will look for aircraft configurations.

If you do not want to modify the Cockpitdecks-provided `config.yaml` file, you always can supply one on the command line with the `—-config` flag.

The set of global parameters provided in the `config.yaml` file is called the Cockpitdecks *environment*, since it provides all « external » information to Cockpitdecks (external, relative to Cockpitdecks).

Cockpitdecks provides two templates configuration files for local and remote use.

## Start Cockpitdecks

Currently, Cockpitdecks only offers a command-line interface to be executed in a shell window.

Installation of the above package in a python environment adds a single command `cockpitdecks-cli` that can be used to start Cockpitdecks and provide options.

### Command-Line Help

If always is a good idea to see what the client application offers:

```
$ cockpitdecks-cli --help
usage: cockpitdecks-cli [-h] [-c config_file] [-d] [-f] [-v] [aircraft_folder]
  
Start Cockpitdecks
  
positional arguments:
  aircraft_folder       aircraft folder for non automatic start
  
options:
  -h, --help            show this help message and exit
  -c config_file, --config config_file
                        alternate configuration file
  -d, --demo            start demo mode
  -f, --fixed           does not automatically switch aircraft
  -v, --verbose         show startup information
```

### Test Cockpitdecks In Demonstration Mode

A second easy step is to start Cockpitdecks in demonstration mode. In this mode, it will offer a single demonstration web deck. See [[Examples]] for details about the demonstration.

```
cockpitdecks-cli --demo
```

In this mode, Cockpitdecks will start a single web deck. Head for the index web page as specified in the `config.yaml` environment file.

![[demo-deck-list.png]]

![[demo-deck.png]]

### Normal Operations

There are two possible ways of working with Cockpitdecks:

1. Either Cockpitdecks runs on the same computer as X-Plane,
2. or Cockpitdecks runs on a remote computer on the same local area network.

> [!NOTE]
> Cockpitdecks provides two templates global parameter/environment files to get you started.

### Cockpitdecks and X-Plane Run On Same Computer

This is the simplest case. In this mode, everything is automatic.

Make sure the configuration file is setup. When everything is run on the same computer, the only important parameter is the X-Plane home folder that should be supplied in `XP_HOME` configuration variable. Alternatively, for convenience, the operating system `XP_HOME` environment variable can be set, especially if it is the only configuration variable that has to be changed. Then issue:

```sh
cockpitdecks-cli
```

Cockpitdecks will immediately start in demonstration mode and listen for X-Plane interaction. If Cockpitdecks detects tht X-Plane is running, finds that an aircraft is loaded, and that a Cockpitdecks `deckconfig` folder exists in the folder of that aircraft, Cockpitdecks will load this configuration.

If no configuration is found, Cockpitdecks will listen and interpret X-Plane data but will not load a new configuration.

This mode is fully automatic, Cockpitdecks always attempts to load the current aircraft `deckconfig` configuration, if present. Similarly, the XPPython3 plugin, if installed, will load the corresponding new configuration as well.

### Cockpidecks and X-Plane Run on Different Computer

In this case, it is not possible for Cockpitdecks to locate aircraft configuration files since they probably are on the remote computer. A set of configuration file will need to be supplied to Cockpitdecks on the command line to start with. (If no folder is supplied, Cockpitdecks will start in demonstration mode. See above.)

```sh
cockpitdecks-cli aircrafts/A21N --fixed
```

Cockpitdecks will start and load the configuration in `aircrafts/A21N/deckcockpit`. Cockpitdecks will attempt to connect to X-Plane.

The optional `--fixed` flag ensure that Cockpitdecks will not reload a new aircraft if it detects the aircraft in X-Plane has changed or does not correspond to the aircraft currently being used.

If `--fixed` is not present, Cockpitdecks will attempt to find a suitable configuration to load. To do this, Cockpitdecks will search for a folder named like the X-Plane aircraft folder. It will search in all folders listed in the `COCKPITDECKS_PATH` python or environment variable.

Here is an example. Suppose you loaded Toliss A321 aircraft in X-Plane. The folder name for that aircraft is `Toliss A321`.

If you defined the python variable as such:

```
COCKPITDECKS_PATH="/XPDATA/CockpitdecksConfig:/Volume0/XPlane/Cockpitdecks/data
```

Cockpitdecks will search in each of the above folders (`/XPDATA/CockpitdecksConfig` and `/Volume0/XPlane/Cockpitdecks/data`) for a folder named `Toliss A321`. It will then check whether that folder contains a `deckconfig` subfolder. If it does, Cockpitdecks will load that configuration. If the folder `/XPDATA/CockpitdecksConfig/Toliss A321/deckconfig` is found, Cockpitdecks will load it.

In all case, Cockpitdecks will repetitively try to connect to X-Plane. If it fails to connect, it infinitely tries again until it succeeds. If not connected, decks will load but no command will be issued to X-Plane and no data will come from X-Plane to update decks.

When Cockpitdecks successfully connects to X-Plane, it refreshes all pages by *reloading* them to reflect the state of the aircraft on the decks.

If Cockpitdecks fails to connect to X-Plane or notices it does no longer receive dataref values from X-Plane, it will again repetitively try to connect to it until it succeeds.

# Troubleshooting

To report an issue with Cockpitdecks, you should always include the `XPPython3.log` file created in the X-Plane folder. Cockpitdecks also create a `cockpitdecks.log` files with more information in the directory you started Cockpitdecks from.

The level of information produced in the file is controlled by the logging level parameter. (info=some information and warnings, debug=a lot of information for debugging purpose, your XPPython3.log file may grow quite large.) The parameter is available at the global plugin level (the entire plugin will report all messages), or can be set at a Cockpitdecks internal module level to pin point issues.

# Termination

Cockpitdecks is designed to terminates cleanly. All requested datarefs monitoring are cancelled, connections are closed, all threads are terminated and joined cleanly. However, it may sometimes take a few seconds before a thread terminates. For example, if a thread is meant to run every 30 seconds, it may be necessary to wait a full 30 seconds before the thread notices the termination request and quits. Longer threads (above 30 seconds or a minute) check periodically for termination request. Cockpitdecks should always terminate cleanly.

If necessary, pressing ++ctrl+c++ several time in the main window will stop Cockpitdecks completely right away.
