Cockpitdecks can be extended in several directions.

# Extension Possibilities

## New Button Appearances

The most common extension will be [[Adding Representations|adding custom Representations]].

A Cockpitdecks developer may add a new type of feedback, very much like the [[Weather]] buttons. Those buttons do not exist in aircrafts, they were created to artificially enhance the simulation experience and ease other parts of the simulation. Similarly, there are endless opportunities to create other types of buttons and materialize them on a deck though Cockpitdecks. In Cockpitdecks, this is performed by mainly creating new *Representations*.

## New Deck Model, New Activations

New deck models may appear. [[Adding New Deck Models|Adding a new deck model]] highlights necessary steps.

In addition of defining a new deck model, it might be necessary to add a new method of interaction that this deck may provide. In this case, it might me necessary to [[Adding Activations|develop specialized, custom activation mechanism]] not foreseen by the current Cockpitdecks version.

## New Simulator Software

Core class [[cockpitdecks-docs/docs/Extending/Simulator]] can also be extended to connect Cockpitdecks to another simulation software. In this case, interaction models imposed by Cockpitdecks (datarefs, commands, UDP connection) inspired by the X-Plane flight simulator software may have to be adjusted as well.

# Extensions Integration

The above descriptions enumerates parts of Cockpitdecks that can be extended. To integrate those extensions into Cockpitdecks, there are two mechanisms.

1. Simple addition of files (for new deck types)
2. Addition of python packages (for code, etc.)

## Adding Custom Deck Types

The first mechanism is limited to simpler extensions like providing new deck types or models. In this case, it is sufficient to create and provide a new Deck Type file. This also applies to defining new web decks.

First, in normal operations, when an aircraft configuration is loaded, Cockpitdecks will search for deck types in the

```
<Aircraft Folder>/deckconfig/resources/decks
```

folder. If new deck types are found, they are loaded and made available.

Second, if a new deck type is available for all aircrafts, it is possible to create a similar folder organisation at another location and to point the cockpitdecks environment variable to it.

```
COCKPITDECKS_EXTENSION_PATH:
  - /Users/xplane/cockpitdecks/mydecktypes
```

On this case, Cockpitdecks will look for a folder named `decks/resources/types` at the above location and load deck types defined there.

## Adding Code

The second extension mechanism requires adding (python) code to Cockpitdecks.

In this case, the extension mechanism proceeds by providing extension packages to Cockpitdecks, either by pointing at a folder that contains the package, or by mentioning its name if the package has been loaded by another method.

Providing extension by location:

```yaml
COCKPITDECKS_EXTENSION_PATH: /Users/xplane/cockpitdekcs/myextension/mypackage
```

will loaded package named `mypackage` from the above folder. (In this case, the above folder path will be added to python `sys.path` and package will be discovered from there.)

Providing extension name:

```yaml
COCKPITDECKS_EXTENSION_NAME: customext
```

will load package `customext`.

Cockpitdecks will recursively load the entire extension packages on startup, discover and add extensions provided by those packages.

Depending on the extension provided, discovery is achieved as follow:

- [[Adding Activations|New activations]] (python classes) needs to be derived from the `cockpitdecks.buttons.activation.Activation` class.
- [[Adding Representations|New representations]] needs to be derived from the `cockpitdecks.buttons.representation.Representation` class.
- [[Adding New Deck Models|New deck]] (python classes) needs to be derived from the `cockpitdecks.deck` class.
- New simulator software needs to be derived from the `cockpitdecks.Simulator` class.

Cockpitdecks will discover daughter classes of the above core classes and add them to its collections of available resources.

In addition to the above code, new deck types can be added like described above, by providing the following structure inside an extension package:

```
<extension-name>.decks.resources.types
```

Resources files located in that module will be loaded as Deck Types.

# Tools for Developers

Cockpitdecks incudes a few [[Button Activations for Developers|developer specific activations]] that can be used to introspect Cockpitdecks while running.
