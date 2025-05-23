Please follow instructions on [[Deck Design]] page to build the correct `deckconfig` folder structure.

# Add a Page

Inside the `cockpit` folder, create a file `index.yaml`. Insert the following content in the file:

```
buttons:
  - index: 0
    label: Map
    type: push
    icon: map
    command: sim/map/show_current
```

# Run

Start X-Plane, load the aircraft where you created your Cockpitdecks configuration folder.

```
$ cockpitdecks-cli --verbose
```

Cockpitdecks will start in demonstration mode, and connect to the simulator. Once it is connected to the simulator, it will try to collect the information about the aircraft that is currently loaded. This may take a few seconds. Once Cockpitdecks has the information about the aircraft currently being used, it will search for the `deckconfig` folder in the folder of the aircraft, and if found, will load the configuration it finds there.

If everything worked properly, you should now see the first, upper left button of your deck displaying the Map text.

# Troubleshooting

Currently, Cockpitdecks is a application started in a command line shell window. When started with the `cockpitdecks-cli` command, Cockpitdecks will produce information and warning messages on the shell window. This is the primary source of information to decode what Cockpitdecks do. The most important information is also written into a `cockpitdecks.log` file.

# More

The above example is simple, yet, everything Cockpitdecks appears in it.

Here are a reasonably complex button definitions:

```
  - index: 26
    name: LANDING LIGHTS
    label: LAND
    type: updown
    commands:
      - toliss_airbus/lightcommands/LLandLightUp
      - toliss_airbus/lightcommands/LLandLightDown
    stops: 2
    switch:
      switch-style: 3dot
      switch-width: 18
      button-fill-color: black
      button-underline-width: 4
      button-underline-color: coral
      tick-labels:
        - "ON"
        - "OFF"
      tick-space: 10
      tick-label-size: 30
      tick-label-font: DIN Bold
    options: hexa
    dataref: AirbusFBW/OHPLightSwitches[4]

```

```
  - index: 0
    name: ICE WING
    label: WING
    type: push
    command: toliss_airbus/antiicecommands/WingToggle
    annunciator:
      model: B
      size: large
      B0:
        text: FAIL
        color: darkorange
        dataref: AirbusFBW/OHPLightsATA30[1]
      B1:
        text: ON
        color: deepskyblue
        framed: true
        dataref: AirbusFBW/OHPLightsATA30[0]
```

## References

[[Button|What is a button?]]

More information on available [[Activations]].

More information on available [[Representations]]
