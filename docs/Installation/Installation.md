Cockpitdecks is an autonomous application that runs next to a simulation software. Cockpitdecks can run on the same computer as the simulator software or on another computer connected to it.

Cockpitdecks is written in the python language. To run it, python language need to be available on the computer where Cockpitdecks will run. It is recommended to run Cockpitdecks application in its own python environment so that it does not interfere with other python applications.

![[installer.jpg|600]]

Installation requires a few steps to be carried over in order:

1. Check the requirements
2. Check the simulation software requirements
3. Install Cockpitdecks
4. Optionally start the demonstration (no simulation software necessary) to confirm the proper installation of Cockpitdecks
5. Install configuration files specific to the aircraft you will use
6. Start your simulation software and Cockpitdecks application

At a later stage, you will learn to create your own configuration files, or even extend Cockpitdecks with new components.

# Requirements

## Hardware Requirements

If you own a desktop deck device, it must be one of these devices:

- [Elgato Stream Deck](https://www.elgato.com/us/en/s/welcome-to-stream-deck) (all models work: Small, Mk.2, XL, Plus, and Neo)
- [Loupedeck Loupedeck Live](https://loupedeck.com/products/loupedeck-live/) (Loupedeck Live S, Loupedeck+, Loupedeck CT should also work but have not been tested; the LoupedeckLive «clone»  [Razer stream controller](https://www.razer.com/mena-en/gaming-accessories/razer-stream-controller) also seems to work.)
- [Behringer X-Touch Mini](https://www.behringer.com/product.html?modelCode=0808-AAF)

## Software Requirements

Cockpitdecks is a python application and will run under python 3.12, or newer. Python language need to be installed and available on the host that will run Cockpitdecks. We highly recommend using the latest python release.

It is recommended to create a dedicated environment and run Cockpitdecks within that environment.

## Simulator Sofware Requirement

Currently, Cockpitdecks only works with Laminar X-Plane simulation software. Here are the requirements for the X-Plane simulator software.

### X-Plane Version

> [!WARNING]
> Recent versions of Cockpitdecks (release 15 and above) **require** version 12.1.4 or later of X-Plane, because Cockpitdecks now exclusively relies on the new X-Plane Web API (release v2 of local Web API, and no longer on UDP). X-Plane Web API considerably simplified communication with the simulator software and made it more reliable. Please make sure you update X-Plane to the latest release before installing and using Cockpitdecks.

If you do not upgrade X-Plane to the latest release, [this older release](https://github.com/devleaks/cockpitdecks/tree/BEFORE-XP120104) may help. Please be aware that this older release will no longer be maintained. It relies on X-Plane UDP API and XPPython3 plugin to circumvent X-Plane UDP API limitations.

### Enable X-Plane Network

Please refer to X-Plane manual to allow for network access to X-Plane.

*Default behavior* as provided with new installations is OK, there is no action necessary.

# Install Cockpitdecks Application

## Install Cockpitdecks Software

Create a new python environment and activate it. In that environment, issue the installation command:

```sh
pip install 'cockpitdecks[xplane,weather,demoext,streamdeck] @ git+https://github.com/devleaks/cockpitdecks.git'
```

### Cockpitdecks Extension Packages

Cockpitdecks is a modular application with extensions. It allows users to only install extensions they require. If you only own Loupedeck devices, there is no need to install Stream Deck or X-Touch Mini extensions.

Valid extension packages (placed between the `[` `]` in the above installation command, comma separated, no space) are:

| Extra         | Content                                                                                                                                                                                     |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `xplane`      | Add X-Plane flight simulator. Mandatory if you want to use it with X-Plane flight simulator.                                                                                                |
| `weather`     | Add special iconic representation for weather. These representations fetch information *outside* of simulation software. For this reason, it is not bundled with Cockpitdecks.              |
| `toliss`      | Highly specific extension for ToLiss Airbus aircrafts in X-Plane flight simulator. (should work with most Airbus models). Do not install if yo do not own one of those.                     |
| `streamdeck`  | For Elgato Stream Deck devices.                                                                                                                                                             |
| `loupedeck`   | For Loupedeck LoupedeckLive, LoupedeckLive.s and Loupedeck CT devices.                                                                                                                      |
| `xtouchmini`  | For Berhinger X-Touch Mini devices.                                                                                                                                                         |
| `demoext`     | Add a few Loupedeck and Stream Deck+ demo extensions. Add a dimmer representation to control the backlight of decks. This extension can be used as a template for creating other extension. |
| `development` | For Cockpitdecks developer only, adds testing packages and python types. Useless if you do not develop Cockpitdecks software.                                                               |

## Test Cockpitdecks Installation — Optional

After installing Cockpitdecks, it is advisable to start the demonstration. It will ensure that Cockpitdecks works properly.

The installation process installs a single command-line instruction called `cockpitdecks-cli` from which all Cockpitdecks operations can be initiated.

To get a glimpse at the possibilities of the client application, issue the following command:

```
cockpitdecks-cli --help
```

Parameters and options will be explained later.

To start the demonstration, at the command prompt, issue the following command:

```
cockpitdecks-cli --demo
```

The demonstration mode does not require a simulator. It provides a single web deck with demonstration interaction.

![[demo-deck-list.png|500]]

![[demo-deck.png|500]]

To exit the demonstration deck, go on the second page, press the guarded button "STOP" for two seconds at least to "lift" the guard. If the guard was lifted, press again the button to stop.

## Install Aircraft Configurations

The determine what desktop deck device will do when manipulated, Cockpitdecks reads a set of configuration files. These configuration files change from aircraft to aircraft. There are therefore aircraft specific.

[Duane](https://github.com/dlicudi), a Cockpitdecks aficionado has realized several configurations for several aircrafts. You can [download them from here](https://github.com/dlicudi/cockpitdecks-configs). For each aircraft type, there will be a folder named `deckconfig`.

For automatic finding of aircraft configuration files, Cockpitdecks expects the `deckconfig` folder *inside* the folder of the aircraft being used, like so:

```
<X-Plane 12 Folder>/Aircraft/Extra Airicraft/Toliss A321/deckconfig
```

If Cockpitdecks does not find a folder named `deckconfig` inside the aircraft folder, it will search for configuration files are other locations, and if none is found, it will not load any configuration and only propose the demonstration.

Now you are ready to [[Starting Cockpitdecks|start Cockpitdecks]].

# Running Cockpitdecks from a Remote Computer

> [!WARNING] Running Cockpitdecks from a Remote Computer
> This section is for experts only.

If you run Cockpitdecks from a remote computer you will need to install a websocket proxy server on the host where X-Plane runs. The reason is that, currently, X-Plane does not allow remote connection to its API. The simplest solution is to setup an `nginx` web server and make it behave like [a HTTP proxy server](https://gist.github.com/devleaks/729bda6db10007b844111178694c7971). The complication will be removed as soon as X-Plane enable its security restriction.

![[2computers.png]]
