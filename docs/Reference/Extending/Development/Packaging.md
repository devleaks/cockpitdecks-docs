Cockpitdecks is «packaged» (in python's terminology) into a few packages to cleanly split dependencies and requirements.

The main package contains Cockpitdecks core engine and virtual web decks.

The following simulator packages are available:

- X-Plane (package named cockpitdecks_xp, named xplane)

The following (physical) deck packages are availalble:

- Streamdeck (package cockpitdecks_sd, named streadeck)
- Loupedeck (package cockpitdecks_ld, named loupedeck)
- Behringer X-Touch Mini (packages cokpitdecks_bx, named xtouchmini)

The following extension packages are also available:

- Weather and METAR (package cockpitdecks_wm, named weather), adds representation to display METAR information as an icon.
- Demo Extension (package cockpitdecks_ext, named demoext), add a few specialized representations, sometimes very dependent on a specific deck model.

A single extension can be installed as such:

```
pip install 'cockpitdecks_ext @ git+https://github.com/devleaks/cockpitdecks_ext.git'
```

where cockpitdecks_ext is the extension package name

or

```
pip install 'cockpitdecks[demoext] @ git+https://github.com/devleaks/cockpitdecks.git'
```

where demoext is the extension name.

Each extension lives its own evolution and maintenance part. Requirements are precisely set and monitored regularly to ensure smooth cooperation between all packages.

> [!NOTE] X-Plane Simulator Extension
> Currently, X-Plane simulator is the only simulator option available. As such, it is included in the core Cockpitdecks installation. It is not necessary to install it separately.

# Note about Deck Extensions

Deck-specific extensions contain both physical deck and virtual web decks. The reason is that some virtual web deck extensions extensively rely on deck device drivers.

# Package and Extension Discovery

There is no black magic in automatic discovery. Packages and extensions are discovered

- either because the extension inherit from a monitored class (example: Observables, Simulator, Deck, Activation, Representation),
- or because configuration files are found at expected rendez-vous location (example `resources/decks`).

## Class Extension

Classes are found because they are loaded and inherit from a formal base class.

## Configuration Extension

New resources are found because they are located in required folder structures. Nous resources are loaded and either added or merged with existing ones.
