![[installer.jpg|600]]

# Disconnect OEM Applications

First, you have to completely stop (quit completely) original manufacturer deck configuration applications. They take exclusive access to the device and that prevents Cockpitdecks from finding and using them.

# Discover Cockpitdecks Application

Cockpitdecks is an application written in a scripting language. It still currently need to be started from a command line prompt.

Cockpitdecks only offers a command-line interface to be executed in a shell window.

Installation of the Cockpitdecks package in a python environment adds a single command `cockpitdecks-cli` that can be used to start Cockpitdecks and provide options.

Cockpitdecks does it best at starting and finding all components it needs automatically. However, it is possible to explicitely specify a parameter Cockpitdecks would not guess or find.

## Command-Line Help

If always is a good idea to see what the client application offers:

```
$ cockpitdecks-cli --help
usage: cockpitdecks-cli [-h] [--version] [-v] [-d] [-f] [-w] [-p PACKAGES [PACKAGES ...]] [--template aircraft folder] [--designer] [aircraft_folder]
  
Start Cockpitdecks
  
positional arguments:
  aircraft_folder       aircraft folder for non automatic start
  
options:
  -h, --help            show this help message and exit
  --version             show version information and exit
  -v, --verbose         show startup information
  -d, --demo            start demo mode
  -f, --fixed           does not automatically switch aircraft
  -w, --web             open web application in new browser window
  -p PACKAGES [PACKAGES ...], --packages PACKAGES [PACKAGES ...]
                        lookup and load additional packages
  --template aircraft folder
                        create deckconfig and add template files to start
                        in supplied aircraft folder
  --designer            start designer
```

## Test Cockpitdecks In Demonstration Mode

A second easy step is to start Cockpitdecks in demonstration mode. It will offer a single demonstration web deck. See [[Demonstration Web Deck]] for details about the demonstration.

```
cockpitdecks-cli --demo
```

In this mode, Cockpitdecks will start a single web deck. Head for the index web page at the following default address:

```
http://127.0.0.1:7777/
```

![[demo-deck-list.png|400]]

![[demo-deck.png|400]]

> [!NOTE] NoSimulator
> If no simulator package is loaded, Cockpitdecks will start a dummy, empty simulator connector for its internal use. The above demonstration will be fully functional. Please note that if no specific deck package has been installed, representation specific to a desktop deck device may not appear correctly on the emulated web decks. If streamdeck package has not been installed, streamdeck specific hardware representations won’t be available on web decks.

# Start Cockpitdecks Application

Start Cockpitdecks without any parameter. Cockpitdecks will do its best at guessing what is available on the computer where it runs. Using the `--vebose` flag will display additional information.

```sh
$ cockpitdecks-cli --verbose
```

Sometimes, or for some complex environment, Cockpitdecks cannot find the information it needs and it will be necessary to supply that information to Cockpitdecks through operating system environment variables.

## Environment Variables

| Variable                      | Definiton                                                                                                                                                                                                   |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `SIMULATOR_NAME`              | Name of simulator to use. Currently, only X-Plane is a valid value.                                                                                                                                         |
| `SIMULATOR_HOME`              | Home directory of X-Plane on the computer where Cockpitdecks runs. If X-Plane is installed on a remote host, `SIMULATOR_HOME` must be `None`.                                                               |
| `SIMULATOR_HOST`              | Hostname or IP address where *X-Plane* runs. Defaults to local host.                                                                                                                                        |
| `APP_HOST`                    | Tuple (Hostname or IP address, port) where *Cockpitdecks* application runs. If specified through operating system environment variables, use (**APP_HOST** and **APP_PORT**). Default to (localhost, 7777). |
| `API_PORT`                    | X-Plane (12.1.1 and above) where REST API runs. Default to `8086`.                                                                                                                                          |
| `API_PATH`                    | X-Plane (12.1.1 and above) where REST API: X-Plane API root path. Currently default to `/api/v1`.                                                                                                           |
| `COCKPITDECKS_EXTENSION_PATH` | List of paths where to search for Cockpitdecks extensions                                                                                                                                                   |
| `COCKPITDECKS_EXTENSION_NAME` | List of python package names that contains Cockpitdecks extensions                                                                                                                                          |

In addition, there is an environment variable `COCKPITDECKS_PATH` that lists folder paths where Cockpitdecks will look for aircraft configurations.

## Forced Configuration

May be the aircraft loaded in X-Plane has no Cockpitdecks configuration but still, a configuration need to be loaded. The command-line interface can help:

```sh
cockpitdecks-cli folder-where-there-is-a-config --fixed
```

The above command will look for a `deckconfig` folder in the folder supplied on the command line. The `--fixed` flag indicates that Cockpitdecks should not try to load another configuration in case the folder loaded (the one supplied on the command line) and the folder of the aircraft used in the simulator do not match.

# Command-Line Arguments and Options

`--version`

Prints Cockpitdecks version and exits.

`--verbose`

Prints additional information when executed.

`--demo`

Starts Cockpitdecks in demonstration mode.

`--fixed`

Loads an initial aircraft configuration and does not change it, even if loading of a new aircraft is detected.

`--web`

Open the default web browser and show available web decks.

`--template aircraft folder

Creates `deckconfig` folder in the supplied aircraft folder and create necessary minimal files from which a new configuration can be created.

If a `deckconfig`folder already exists in the supplied aircraft folder, no template is installed.

`--packages PACKAGE [PACKAGE..]`

Search and load supplied extension packages on startup. Mainly used for development purposee.

`--designer`

Starts the web deck designer (expert only).

# Troubleshooting

Cockpitdecks creates a `cockpitdecks.log` files with more information in the directory you started Cockpitdecks from.

The level of information produced in the file is controlled by the logging level parameter. (info=some information and warnings, debug=a lot of information for debugging purpose, your XPPython3.log file may grow quite large.) The parameter is available at the global plugin level (the entire plugin will report all messages), or can be set at a Cockpitdecks internal module level to pin point issues.

Cockpitdecks is *stateless*. If we except its internal statistics, Cockpitdecks does not maintain any state variable of a situation. Therefore, at any time, it is possible to stop and restart it. Should a problem persists, please file an issue on [Github](https://github.com/devleaks/cockpitdecks/issues) with necessary information to reproduce it.

# Termination

To terminate Cockpitdecks, press **a single** ++ctrl+c++ to stop Cockpitdecks client application.

Cockpitdecks is designed to terminates cleanly. All requested datarefs monitoring are cancelled, connections are closed, all threads are terminated and joined cleanly. However, it may sometimes take a few seconds before a thread terminates. For example, if a thread is meant to run every 30 seconds, it may be necessary to wait a full 30 seconds before the thread notices the termination request and quits. Longer threads (above 30 seconds or a minute) check periodically for termination request. Cockpitdecks should always terminate cleanly to release resources it controls from the simulator.

If necessary, there is an [[Activations for Developers#Stop|activation]] that can be assigned to a button to stop Cockpitdecks.

If necessary, or blocked, pressing ++ctrl+c++ several time in the main window will stop Cockpitdecks completely right away.
