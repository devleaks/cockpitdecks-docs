This page is an attempt at providing all necessary step to create a new page of buttons to display on a deck.

We assume you installed Cockpitdecks properly, together with the Cockpitdecks Helper Plugin which is necessary to work in fully automatic mode.

# Deck Preparation

First, make sure the deck model you own is a deck already available through Cockpitdecks. The following deck brands and models have been tested and used:

- Elgato Strem Deck: Streamdeck MK.2, Streamdeck XL, Streamdeck Mini, Streamdeck+, Streamdeck Neo
- Loupedeck: LoupedeckLive, LoupedeckLive.s
- Behringer: X-Touch Mini (I recently noticed that there are several version of the same device, but they should all work since the Makie mode is used.)

## Prepare the Folder

In normal, simple, automatic mode, Cockpitdecks expects configuration to be found in the folder of the aircraft currently being used. (This may be adjusted by advanced configuration parameters if you do not want to store Cockpitdecks configuration files in the that folder.)

In the aircraft folder, create a folder named `deckconfig`.

## Add the main `config.yaml`File

Inside the above folder, create the main configuration file with this content. Adjust the content to your deck.

```
aircraft: Cessna 172 
decks:
  - name: Aviator Loupdeck
    type: loupedeck
    layout: cockpit
    brightness: 70
```

## Create a layout Folder

In the conifiguration file, you mention the name of the layout you are going to use for that deck: `cockpit`. Inside the deckconfig folder, create a sub-folder and name it `cockpit`.

## Add a Page

Inside that `cockpit` folder, create a file `index.yaml`. Insert the following content in the file:

```
buttons:
  - index: 0
    label: Map
    type: push
    icon: map
    command: sim/map/show_current
```

## First Run

Start X-Plane, load the aircraft where you created your Cockpitdecks configuration folder.

Disconnect all software that access your deck.

### Create an Environment File

If every runs on the same computer, you only need to supply X-Plane home directory. You can supply the folder either through a operating system environment variable `SIMULATOR_HOME`, or through the Cockpitdecks environment file. A `myenviron.yaml` file with the following content is sufficient:

```
SIMULATOR_NAME: X-Plane
SIMULATOR_HOME: /Applications/X-Plane 12
```

### Start Cockpitdecks

```
$ cockpitdecks-cli --env myenviron.yaml
```

Cockpitdecks will start immediately in demonstration mode, and connect to the simulator. Once it is connected to the simulator, it will try to collect the information about the aircraft that is currently loaded. This may take a few seconds. Once Cockpitdecks has the information about the aircraft currently being used, it will search for the deckconfig folder in the folder of the aircraft, and if found, will load the configuration it finds there.

If everything worked properly, you should now see the first, upper left button of your deck displaying the Map text. With no map icon.

## Adding Custom Icons

# Add buttons
