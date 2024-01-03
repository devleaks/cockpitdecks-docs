
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

# Notes About Configuration Values Melting

Sometimes, configuration values can be specified at different level for a given entity.
## Cockpitdecks (Application), Cockpits

At the highest level, a Cockpit will start with a set of default values provided in its code.
It will then loads additional parameters in a global resource configuration file.

## Decks and Layouts

A Deck will start with the configuration attributes supplied by the Cockpit. The Cockpit uses the configuration passed in the the global, aircraft-level configuration file.

A deck will later read an optional configuration file located in the folder of the Layout it will use.
The attributes specified in the layout configuration file will take precedence over those at the deck level.

## Pages and Includes

In addition to button definitions, a Page contains other page-level attributes.

Since a page can include other pages, the included page attributes will **overwrite** the attributes of the page.
