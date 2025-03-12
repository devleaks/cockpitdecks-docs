![[installer.jpg|600]]

# Disconnect OEM Applications

First, you have to completely stop (quit completely) original manufacturer deck configuration applications. They take exclusive access to the device and that prevents Cockpitdecks from finding and using them.

# Start Cockpitdecks

Currently, Cockpitdecks only offers a command-line interface to be executed in a shell window.

Installation of the above package in a python environment adds a single command `cockpitdecks-cli` that can be used to start Cockpitdecks and provide options.

## Command-Line Help

If always is a good idea to see what the client application offers:

```
$ cockpitdecks-cli --help
usage: cockpitdecks-cli [-h] [--version] [-d] [--template template_file] [-f] [-v] [-p PACKAGES [PACKAGES ...]] [aircraft_folder]
  
Start Cockpitdecks
  
positional arguments:
  aircraft_folder       aircraft folder for non automatic start
  
options:
  -h, --help            show this help message and exit
  --version             show version information and exit
  -d, --demo            start demo mode
  --template template_file
                       create deckconfig and add template files to start in supplied aircraft folder
  -f, --fixed           does not automatically switch aircraft
  -v, --verbose         show startup information
  -p PACKAGES [PACKAGES ...], --packages PACKAGES [PACKAGES ...]
                        lookup for optional packages
```

## Test Cockpitdecks In Demonstration Mode

A second easy step is to start Cockpitdecks in demonstration mode. In this mode, it will offer a single demonstration web deck. See [[Examples]] for details about the demonstration.

```
cockpitdecks-cli --demo
```

In this mode, Cockpitdecks will start a single web deck. Head for the index web page at the following default address:

```
http://127.0.0.1:7777/
```

![[demo-deck-list.png]]

![[demo-deck.png]]

> [!NOTE] NoSimulator
> If no simulator package is loaded, Cockpitdecks will start a dummy, empty simulator connector for its internal use. The above demonstration will be fully functional. Please note that if no specific deck package has been loaded, "hardware" reprensentation may not appear correctly on the emulated web decks.

## Start the Cockpitdecks Application

Finally, you can try to launch Cockpitdecks without any parameter. Cockpitdecks will do it best at guessing what is available on the computer where it runs. Using the `--vebose` flag will display additional information.

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

May be thhe aircraft loaded in X-Plane has no Cockpitdecks configuration but still, a configuration need to be loaded. The command-line interface can help:

```sh
cockpitdecks-cli folder-where-there-is-a-config --fixed
```

The above command will look for a `deckconfig` folder in the folder supplied on the command line. The `--fixed` flag indicates that Cockpitdecks should not try to load another configuration in case the folder loaded (the one supplied on the command line) and the folder of the aircraft used in the simulator do not match.

# Troubleshooting

Cockpitdecks creates a `cockpitdecks.log` files with more information in the directory you started Cockpitdecks from.

The level of information produced in the file is controlled by the logging level parameter. (info=some information and warnings, debug=a lot of information for debugging purpose, your XPPython3.log file may grow quite large.) The parameter is available at the global plugin level (the entire plugin will report all messages), or can be set at a Cockpitdecks internal module level to pin point issues.

Cockpitdecks is *stateless*. If we except its internal statistics, Cockpitdecks does not maintain any state variable of a situation. Therefore, at any time, it is possible to stop and restart it. Should a problem persists, please file an issue on [Github](https://github.com/devleaks/cockpitdecks/issues) with necessary information to reproduce it.

# Termination

To terminate Cockpitdecks, press **a single** ++ctrl+c++ to stop Cockpitdecks client application.

Cockpitdecks is designed to terminates cleanly. All requested datarefs monitoring are cancelled, connections are closed, all threads are terminated and joined cleanly. However, it may sometimes take a few seconds before a thread terminates. For example, if a thread is meant to run every 30 seconds, it may be necessary to wait a full 30 seconds before the thread notices the termination request and quits. Longer threads (above 30 seconds or a minute) check periodically for termination request. Cockpitdecks should always terminate cleanly to release resources it controls from the simulator.

If necessary, there is an [[Button Activations for Developers#Stop|activation]] that can be assigned to a button to stop Cockpitdecks.

If necessary, or blocked, pressing ++ctrl+c++ several time in the main window will stop Cockpitdecks completely right away.
