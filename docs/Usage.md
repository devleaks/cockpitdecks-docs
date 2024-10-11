![[installer.jpg|600]]

# Disconnect OEM Applications

First, you have to completely stop (quit completely) original manufacturer deck configuration applications. They take exclusive access to the device and that prevents Cockpitdecks from finding and using them.

# Adjust `environment.yaml`

Cockpitdecks uses a single configuration file, called the Cockpitdecks environment file, to define a few elements that cannot easily be guessed. Cockpitdecks provides a configuration file that is suitable for single computer installation. You must, however, adjust at least the simulator folder path.

| Variable                      | Definiton                                                                                                                                                                                                     |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `SIMULATOR_NAME`              | Name of simulator to use. Currently, only X-Plane is a valid value.                                                                                                                                           |
| `SIMULATOR_HOME`              | Home directory of X-Plane on the computer where Cockpitdecks runs. If X-Plane is installed on a remote host, `SIMULATOR_HOME` must be `None`.                                                                 |
| `SIMULATOR_HOST`              | Hostname or IP address where *X-Plane* runs. Defaults to local host.                                                                                                                                          |
| `APP_HOST`                    | Tuple (Hostname or IP address, port) where *Cockpitdecks* application runs. If specified through operating system environment variables, use (**APP_HOST** and **APP_PORT**). Default to (local host, 7777).  |
| `API_PORT`                    | X-Plane (12.1.1 and above) where REST API runs. Default to `8086`.                                                                                                                                            |
| `API_PATH`                    | X-Plane (12.1.1 and above) where REST API: X-Plane API root path. Currently default to `/api/v1`.                                                                                                             |
| `COCKPITDECKS_EXTENSION_PATH` | List of paths where to search for Cockpitdecks extensions                                                                                                                                                     |
| `COCKPITDECKS_EXTENSION_NAME` | List of python package names that contains Cockpitdecks extensions                                                                                                                                            |

In addition, there is a operating system environment variable `COCKPITDECKS_PATH` that holds folders where Cockpitdecks will look for aircraft configurations.

If you do not want to modify the Cockpitdecks-provided `environ.yaml` file, you always can supply one on the command line with the `—-env` flag.

The set of global parameters provided in the `environ.yaml` file is called the Cockpitdecks *environment*, since it provides all « external » information to Cockpitdecks (external, relative to Cockpitdecks).

Cockpitdecks provides two templates configuration files for local and remote use.

# Start Cockpitdecks

Currently, Cockpitdecks only offers a command-line interface to be executed in a shell window.

Installation of the above package in a python environment adds a single command `cockpitdecks-cli` that can be used to start Cockpitdecks and provide options.

## Command-Line Help

If always is a good idea to see what the client application offers:

```
$ cockpitdecks-cli --help
usage: cockpitdecks-cli [-h] [-e environ_file] [-d] [-f] [-v] [aircraft_folder]

Start Cockpitdecks

positional arguments:
  aircraft_folder       aircraft folder for non automatic start

options:
  -h, --help            show this help message and exit
  -d, --demo            start demo mode
  -e environ_file, --env environ_file
                        alternate environment file
  -f, --fixed           does not automatically switch aircraft
  -v, --verbose         show startup information
```

## Test Cockpitdecks In Demonstration Mode

A second easy step is to start Cockpitdecks in demonstration mode. In this mode, it will offer a single demonstration web deck. See [[Examples]] for details about the demonstration.

```
cockpitdecks-cli --demo
```

In this mode, Cockpitdecks will start a single web deck. Head for the index web page as specified in the `environ.yaml` environment file (`APP_HOST`).

![[demo-deck-list.png]]

![[demo-deck.png]]

## Normal Operations

There are two possible ways of working with Cockpitdecks:

1. Either Cockpitdecks runs on the same computer as X-Plane,
2. or Cockpitdecks runs on a remote computer on the same local area network.

> [!NOTE]
> Cockpitdecks provides two templates global parameter/environment files to get you started.

## Cockpitdecks and X-Plane Run On Same Computer

This is the simplest case. In this mode, everything is automatic.

Make sure the configuration file is setup. When everything is run on the same computer, the only important parameter is the X-Plane home folder that should be supplied in `SIMULATOR_HOME` configuration variable. Alternatively, for convenience, the operating system `SIMULATOR_HOME` environment variable can be set, especially if it is the only configuration variable that has to be changed. Then issue:

```sh
cockpitdecks-cli
```

Cockpitdecks will immediately start in demonstration mode and listen for X-Plane interaction. If Cockpitdecks detects tht X-Plane is running, finds that an aircraft is loaded, and that a Cockpitdecks `deckconfig` folder exists in the folder of that aircraft, Cockpitdecks will load this configuration.

If no configuration is found, Cockpitdecks will listen and interpret X-Plane data but will not load a new configuration.

This mode is fully automatic, Cockpitdecks always attempts to load the current aircraft `deckconfig` configuration, if present. Similarly, the XPPython3 plugin, if installed, will load the corresponding new configuration as well.

## Cockpidecks and X-Plane Run on Different Computer

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

Cockpitdecks is *stateless*. If we except internal statistics, Cockpitdecks does not maintain any state variable of a situation. Therefore, at any time, it is possible to stop and restart it. Should a problem persists, please file an issue on github with necessary information to reproduce it.

# Termination

Cockpitdecks is designed to terminates cleanly. All requested datarefs monitoring are cancelled, connections are closed, all threads are terminated and joined cleanly. However, it may sometimes take a few seconds before a thread terminates. For example, if a thread is meant to run every 30 seconds, it may be necessary to wait a full 30 seconds before the thread notices the termination request and quits. Longer threads (above 30 seconds or a minute) check periodically for termination request. Cockpitdecks should always terminate cleanly.

If necessary, there is an [[Button Activations for Developers#Stop|activation]] that can be assigned to a button to stop Cockpitdecks.

If necessary, pressing ++ctrl+c++ several time in the main window will stop Cockpitdecks completely right away.
