![[installer.jpg|600]]

Cockpitdecks is a regular python application and will run under python 3.10, or newer.

It is recommended to create a dedicated environment and run Cockpitdecks within that environment.

# X-Plane *!*

Cockpitdecks is a modern software that takes benefit from all available possibilities. While being developed, it is constantly updated to use the latest packages and features.

Therefore, Cockpitdecks will work better with the latest production release of X-Plane. It may not work properly, or some feature may not be available when using older versions of X-Plane. We do not have the engineering resources to maintain working versions of Cockpitdecks for all versions of X-Plane.

Same occurs with aircrafts. Aircrafts are pieces of software that are regularly updated. It is a good practice to include the X-Plane and aircraft version information as comments in the `deckconfig` files, to precise which version of X-Plane or an aircraft is required to run properly.

As a practical example, X-Plane recently opened access to internal data through a new channel: [A Web API access](https://developer.x-plane.com/article/x-plane-web-api/). This is offered in X-Plane release 12.1.4 or newer. Immediately, Cockpitdecks has taken benefit from this new, simplified mean to access simulator values.

It is good practice to maintain the software you use to the latest, production version. Cockpitdecks is no exception to this advise.

If you do not upgrade X-Plane to the latest release, [this older release](https://github.com/devleaks/cockpitdecks/tree/BEFORE-XP120104) may help. Please be aware that this older release will no longer be maintained. It relies on X-Plane UDP API and XPPython3 plugin to circumvent X-Plane UDP API limitations.

## Enable X-Plane Network

(See X-Plane manual. Will provide information here later. *Default behavior* is OK, there is no action necessary.)

# Install Cockpitdecks Application

## Install Cockpitdecks Software

Create a new python environment and activate it. In that environment, issue the pip install command:

```sh
pip install 'cockpitdecks[xplane,weather,demoext,streamdeck] @ git+https://github.com/devleaks/cockpitdecks.git'
```

### Cockpitdecks Extension Packages

Valid installable extension packages (between the `[` `]`, comma separated, no space) are:

| Extra         | Content                                                                                                                                                                                                        |
| ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `xplane`      | Add X-Plane flight simulator. Mandatory if you want to use it with X-Plane flight simulator.                                                                                                                   |
| `weather`     | Add special iconic representation for weather. These representations fetch information outside of simulation software. That's why it is not bundled with Cockpitdecks. Recommended.                            |
| `toliss`      | Representations highly specific to X-Plane ToLiss Airbus aircrafts (should work with most of them)                                                                                                             |
| `streamdeck`  | For Elgato Stream Deck devices                                                                                                                                                                                 |
| `loupedeck`   | For Loupedeck LoupedeckLive, LoupedeckLive.s and Loupedeck CT devices                                                                                                                                          |
| `xtouchmini`  | For Berhinger X-Touch Mini devices                                                                                                                                                                             |
| `demoext`     | Add a few Loupedeck and Stream Deck+ demo extensions. Recommended. Add a dimmer representation to control the backlight of decks. This extension can be used as a template for creating other representations. |
| `development` | For Cockpitdecks developer only, adds testing packages and python types. Useless if you do not develop Cockpitdecks software.                                                                                  |

## Install Aircraft  `deckconfig` Folders

[Duane](https://github.com/dlicudi), a Cockpitdecks aficionado has realized several configurations for several aircrafts.

You can [find them here](https://github.com/dlicudi/cockpitdecks-configs), download them and install them in your aircraft folder.

Cockpitdecks `deckconfig` folder must be placed in the folder of the X-Plane aircraft to be found by Cockpitdecks or the Cockpitdecks Helper plugin.

```
<X-Plane 12 Folder>/Aircraft/Extra Airicraft/Toliss A321/deckconfig
```

Now you are ready to [[Usage|start Cockpitdecks]].

# Running Cockpitdecks from a Remote Computer

If you run Cockpitdecks from a remote computer you will need to install a websocket proxy server on the host where X-Plane runs. The reason is that, currently, X-Plane does not allow remote connection. The simplest solution is to setup an `nginx` web server and make it behave like [a HTTP proxy server](https://gist.github.com/devleaks/729bda6db10007b844111178694c7971).

![[2computers.png]]
