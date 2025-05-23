Cockpitdecks is split into a core package, called `cockpitdecks`, and a few additional optional packages.

# Flight Simulator Packages

`cockcpitdecks_xp` - xplane: [[Laminar Research X-Plane]] flight simulator Extension

> [!NOTE] Title
> `cockpitdecks_xp` is the name of the development package (python). xplane is the short name of the extension to be used when installing.

`cockpitdecks_fs`: [[Microsoft Flight Simulator]] Extension

# Deck Device Packages

`cockpitdecks_bx` - xtouch: Extension to interface with Berhinger X-Touch Mini devices

`cockpitdecks_ld` - loupedeck: Extension to interface with Loupedeck decks

`cockpitdecks_sd` - streamdeck: Extension to interface with Elgato Stream decks

`cockpitdecks_ww` - winwing: Extension to interface with Winwing MCDU keyboard and display

# Extensions Packages

`cockpitdecks_wm` - weather: Extension to display weather information from sources external to the simulation.

`cockpitdecks_ext` - ext: Demonstration extension, mainly for developer, to illustrate extension mechanisms. The proposed extension is a back light dimmer for decks that support back light adjustment.

# Special Purpose Packages

`cockpitdecks_tl`: Extension for [ToLiss Airbus](https://toliss.com) aircrafts simulated in the X-Plane flight simulator.

# Installation of Packages

Packages can be installed be supplying the package name between the `[` `]`:

```sh
pip install 'cockpitdecks[xplane,weather,demoext,streamdeck] @ git+https://github.com/devleaks/cockpitdecks.git'
```
