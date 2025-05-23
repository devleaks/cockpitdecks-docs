The heart of Cockpitdecks is the Deck. A deck is a desktop device with buttons that allow interaction with the computer.

Cockpitdecks configuration is the process of telling the computer, or in our case, the simulation software, what to do when buttons are pressed or otherwise manipulated.

In return, Cockpitdecks will also display information coming from the simulation software on deck devices that allow it. Some deck devices have simple LED light that can be turned on or off, some more complex devices have small LCD/LED/TFT color screens on keys to display small images.

Cockpitdecks allows all this to happen in a highly customizable way through its configuration files.

All configuration files are Yaml-formatted files. Yaml is a simple, highly readable structured text file. It allows for comments inside the file, which is very convenient.

As an alternative to desktop (physical) decks, Cockpitdecks also offer (virtual) web decks. A Web Deck is a deck run in a browser window. No device necessary. Please head here to discover about [[Web Decks]].

# Configuration File Organisation

The overall structure of configuration files is as follow:

``` hl_lines="8"
XPlaneAircraftFolder
  (...)
  ⊢ deckconfig
    config.yaml
    secret.yaml
    ⊢ resources
      ⊢ fonts
      ⊢ icons
      ⊢ sounds
      ⊢ ...
    ⊢ layout1
      ⊢ index.yaml
      ⊢ page2.yaml
      ...
    ⊢ layout2
    ...
```

## config.yaml

The file named `config.yaml` directly inside the `deckconfig` folder is the *main configuration file*. It lists all decks Cockpitdecks will manage, and global parameters.

The main configuration file may be accompanied by a `secret.yaml` file. This additional file contains the serial number of the deck devices, and is only necessary if there are several decks of the same type, to allow for distinction between them.

## resources Folder

The `resources` folder contains resources used by deck devices. It contains icon images, fonts, sounds, or other parameters.

Please refer to the [[Reference/Resources|reference documentation]] for a formal presentation of the `resources` folder content.

## Layout Folders

Layout folders are used to group all things related to one precise deck. In the main configuration file that lists all deck devices that will be used, each deck device will have a mandatory layout folder assigned to it. That is where the deck will find its own particular configuration files and be able to determine what it has to do and display when used.

### Page

The layout folder contains at least one file, called a Page. The default name of the file that contains the first Page is `index.yaml`.

It is inside this file that the deck will discover what is has to do and display.

Here is the begining of such a index Page:

```
buttons:
  - index: 0
    type: push
    command: sim/view/map_toggle
    icon: map
  - ...
```

This define that the key (button) at index 0 (top, left) has to issue the command `sim/view/map_toggle` when pressed, and display the icon image named `map` on the key.

This is a very simple, basic example, but it shows what need to be done to tell each button of each deck what to do and what to display when used with Cockpitdecks.

# Adding Decks

The first step is to add decks Cockpitdecks will manage in Cockpitdecks main configuration file:

```
decks:
  - name: XPLive
    type: LoupedeckLive
    layout: cockpit
    brightness: 100
```

Cockpitdecks will manage a LoupedeckLive deck. We will name that deck XPLive when we refer to it. The deck will find its configuration in the layout folder named `cockpit`.

The backlight of the deck will be set to its 100(%) maximum. It could be dimmed to a lower value for night flights.

# Adding Deck Pages

Inside `deckconfig`folder, we create a layout folder named `cockpit`.

Inside that `cockpit` folder, we create a file named `index.yaml`.

The content of the file defines what the LoupedeckLive deck will do when keys are pressed, or dials turned, and what the deck will need to display in return.

[[Adding a Page of Buttons to a Deck]]
