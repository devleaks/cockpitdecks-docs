
# Button Attribute Values

Buttons are defined by a list of attributes.

Some attributes are unique to a button, very specific, and cannot have default values like, for exemple, the button index or a button label attribute.

Some other attributes are used by numerous buttons, like for example the *color* of the label. These common attributes benefit from a sophisticated default value lookup.
# Attribute Hierarchical Lookup

Before we can look up at the attribute evaluation method, we must notice that all objects we manipulate in Cockpitdecks are hierarchically organized.

1. A the highest, top level sits the Cockpit.
2. The Cockpit holds all Decks available to the simulator.
3. Each Deck uses a Layout
4. Each Layout has one or more Pages
5. Each Page can include other Pages
6. Each Page contains all Buttons definitions.

This hierarchy is very important.

As an example, let us find the value of the `label-color`.
First, the button will perform a direct lookup in its attribute. If it finds a  `label-color` it will use it. If it does not find it, it will as its `default-label-color`. The button will then ask its parent entity for the `default-label-color`.

So the Page will search for a `default-label-color` in its attributes. If it finds it, it will use it. If it does not find it, it ask its parent entity.

The Deck will search for a `default-label-color` in its attributes. If it finds it, it will use it. If it does not find it, it ask its parent entity.

Finally, the Cockpit will return a `default-label-color`. If there is no value for the `default-label-color`, Cockpitdecks will issue an error. It simply means that there is no default value for that attribute and that a value *must* be supplied by the user in the definition of the button.

# Day / Night (or light / dark) Theme

Cockpitdecks attempts to provide a day and a night *theme*. The attribute `cockpit` can be set to `day` (or `light`) or `night`(or `dark`).

```yaml
cockpit-theme: dark
```

The effect is that in `night` (or `dark`) theme, default values prefixed with `dark-` will be favored. If no default value prefixed with `dark-` is found, the regular default value is fetched.

Example for label color:

In `day` mode, the default value for label color is `default-label-color`, as usual.

In `night` mode, the default value `dark-default-label-color` value is first searched, and if not defined, the `default-label-color` is retuned.

> [!NOTE]
> Ultimately, this scheme can be extended to any theme name value, like airbus, or barbie. However, it is advisable to limit theme default values to global appearance parameters like colors, fonts, textures, and sizes.

# Configuration Files

1. Cockpitdecks internals (cannot be modified)
2. Global configuration file: Cockpitdecks/resources/config.yaml
3. Aircraft configuration file: Aircraft/config.yaml
4. Layout configuration file: Aircraft/layout1/config.yaml
5. Page configuration file: Aircraft/layout1/page1.yaml

# Notes About Configuration Values Melting

Sometimes, configuration values can be specified at different level for a given entity.
## Cockpitdecks (Application), Cockpits

At the highest level, a Cockpit will start with a set of default values provided in its code.

It will then loads additional parameters in a global resource configuration file. That file is the same for ALL aircrafts. The config.yaml file is located in the home directory of Cockpitdecks software, in the *resources* folder.

Next, Cockpitdecks will look for an aircraft specific configuration file, in the  deckconfig folder of that aircraft. It will load the config.yaml file of that aircraft, and default values loaded from there will apply to that aircraft only.

## Decks and Layouts

A Deck will start with the configuration attributes supplied by the Cockpit. The Cockpit uses the configuration passed in the the global, aircraft-level configuration file.

A deck will load a Layout. When doing so, the deck may read an optional configuration file located in the folder of the Layout it will use. The attributes specified in the layout configuration file will take precedence over those at the deck level.

## Pages and Includes

In addition to button definitions, a Page contains other page-level attributes.

When a page includes another page, their respective attributes get *melted* (combined). The attributes of the included page **overwrite** the attributes of the base page.

Since a page can include more than one other page, the attributes of the included page are added (on top of) the attribute of the base page and other included pages. But since the order of page inclusion is not specified, attributes may be piled up in any order.

In other words, it is advisable to not include any page-level attribute in a page that will be included in another page, it may lead to unexpected behavior or presentation. It is safer to limit inclusion to the `buttons` attribute, where buttons of main page and included page are merged together.