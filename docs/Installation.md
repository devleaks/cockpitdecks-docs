![[installer.jpg|600]]

Cockpitdecks is a regular python application and will run with python 3.10, or newer.

It is recommended to create a dedicated environment and run Cockpitdecks within that environment.

# X-Plane *!*

Cockpitdecks is a modern software that takes benefit from all available possibilities. While being developed, it is constantly updated to use the latest packages and features.

Therefore, Cockpitdecks will work better with the latest production release of X-Plane. It may not work properly, or some feature may not be available when using older versions of X-Plane. We do not have the engineering resources to maintain working versions of Cockpitdecks for all versions of X-Plane. While most features should still work in X-Plane 11, they were never tested, and we will not provide any support to make it work on older release.

Same occurs with aircrafts. Aircrafts are pieces of software that are regularly updated. It is a good practice to include the X-Plane and aircraft version information as comments in the `deckconfig` files, to precise which version of X-Plane or an aircraft is required to run properly.

As a practical example, X-Plane recently opened access to internal data through a new channel: [A Web REST API access](https://developer.x-plane.com/article/x-plane-web-api/). This is offered in X-Plane release 12.1.1 or newer. Immediately, Cockpitdecks has taken benefit from this new, simplified mean to access internal values.

It is good practice to maintain the software you use to the latest, production version. Cockpitdecks is no exception to this advise.

In particular, the X-Plane Cockpitdecks Helper plugin uses and requires the latest version of [XPPython3](https://xppython3.readthedocs.io/en/latest/index.html) plugin to work. This plugin itself requires X-Plane 12 and recent version of X-Plane SDK. Cockpitdecks Helper plugin is strictly not required to run Cockpitdecks but some features will not work without it.

## Enable X-Plane UDP

> [!NOTE] X-Plane 12 and UDP
> X-Plane 12 appears to disable UDP networking initially. Check Settings -> Network page, and make sure “Accept incoming connections” is enabled.
 
(See X-Plane UDP manual. Will provide information here later.)

# Install Cockpitdecks Application

## Install Cockpitdecks Software

Create a new python environment and activate it. In that environment, issue the pip install command:

```sh
pip install 'cockpitdecks[weather,streamdeck] @ git+https://github.com/devleaks/cockpitdecks.git'
```

Valid installable extras (between the `[` `]`, comma separated, no space) are:

| Extra              | Content                                                                                                                     |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------- |
| `cockpitdecks_ext` | Load Stream Deck, Loupedeck, and X-Touch capabilities. Highly recommended, almost mandatory.                                |
| `weather`          | To load special iconic representation for weather. These icons sometimes fetch information outside of X-Plane. Recommended. |
| `streamdeck`       | For Elgato Stream Deck decks                                                                                                |
| `loupedeck`        | For Loupedeck LoupedeckLive, LoupedeckLive.s and Loupedeck CT devices                                                       |
| `xtouchmini`       | For Berhinger X-Touch Mini device                                                                                           |
| `development`      | For developer only, add testing packages and python types                                                                   |

## Install Cockpitdecks Helper Plugin

> [!WARNING] Cockpitdecks X-Plane Helper Plugin
> You can do this step later, but some functions will not work or be available inside Cockpitdecks.

X-Plane UDP has some shortcomings that prevent some operations with decks.

To circumvent this, Cockpitdecks provides a small python plugin called the Cockpitdecks Helper plugin, that need to be installed into X-Plane. The Cockpitdecks Helper plugin will execute some instructions on behalf of the Cockpitdecks application. Cockpitdecks Helper plugin just need to be installed and will provide its services to Cockpitdecks.

If not installed, some of the commands inside Cockpitdecks will work properly.

Cockpitdecks Helper plugin is an _in-process_ plugin, running inside X-Plane, while Cockpitdecks is an _out-of-process_ executable.

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

## Install Aircraft Specific `deckconfig` Folders

Duane, a Cockpitdecks aficionado has realized several configurations for several aircrafts.

You can [find them here](https://github.com/dlicudi/cockpitdecks-configs), download them and install them in your aircraft folder.

Cockpitdecks `deckconfig` folder must be placed in the folder of the X-Plane aircraft to be found by Cockpitdecks or the Cockpitdecks Helper plugin.

```
<X-Plane 12 Folder>/Aircraft/Extra Airicraft/Toliss A321/deckconfig
```

Now you are ready to [[Usage|start Cockpitdecks]].
