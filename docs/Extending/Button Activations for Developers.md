Cockpitdecks offers a few special activations that are normally not called in regular use.

# Reload

`type: reload`

No option. Provoke the reload of all decks from initialisation.

The procedure first gracefull terminates the current setup, and then reloads the setup.

If a page different than the home page was currently loaded, the process will try to reload the same page if available in the new setup.

This activation is mainly used during development process to create buttons, alter their appearance and re-load the configuration to see the changes.

(Please note that reloading decks is a complex process, since it involves the reset of all devices, reloading all configurations, and displaying pages as they used to be, if still present.)

# Stop

`type: stop`

No option. Gracefully stops all decks and terminates Cockpitdecks.

# Inspect

`type: inspect`

Provoke the output of information for all buttons of all pages or all decks.

Mainly for development purpose.

Inspection always starts at the Cockpit level and may or may not crawl down into its constituting parts like decks, layouts, pages, and buttons. Some inspection terminates earlier in the drill down. For example, `threads` inspection stops at the Deck level.

## Attribute

Button Inspect has one attribute

`what`

The value of the `what` attribute determine what is displayed when activated.

### what Attribute Values

| Value               | Description                                                                                                                                                                    |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `threads`           | list Cockpitdecks and deck threads that are currently running.                                                                                                                 |
| `datarefs`          | list datarefs and values (the page containing the buttons need to be loaded first before it can display values).                                                               |
| `datarefs-listener` | List all datarefs and which buttons (listeners) are using the dataref.                                                                                                         |
| `status`            | list internal variables and statuses of each button.                                                                                                                           |
| `valid`             | check button validity and report invalid status.                                                                                                                               |
| `desc`              | print a description of what each button does in plain English, both for activation and representation.                                                                         |
| `image`             | produces an image of each deck, images are saved in the Cockpitdecks home directory and named after the deck.                                                                  |
| `longpress`         | list [[Button Activation#ShortOrLongpress]] command that should have a couple of additional commands [[X-Plane Interaction#Activations\|added to X-Plane through the plugin]]. |

```Yaml
index: 4
type: inspect
what: desc
```

When button index 4 is pressed, it will display what each button does (description) in plain English on the output or debugging screen or file.

# Notes for Button Designers

## Button Instantiation

When a button is created, internal meta data are set first. Second, the Activation is installed and initialised. Third, the Representation is installed and initialized, as it may already use some activation information for rendering. Finally, the button is initialised. It will be rendered when the page that contains it is loaded on a deck.

## Button Validity

Each button has a validity function that ensures that all necessary attributes are provided in its definition. If the activation of the button is not valid, its activation function will never be triggered, because of missing or misconfigurated parameters. If its representation is not valid, it will not be rendered on the deck.

If a button is not valid, a small red triangle appears in the lower right corner of the key icon if the button is capable of representing it. A small blue triangle appears in the lower right corner of the key icon if Cockpitdecks suspect the button is a placeholder.

## Button Description and Inspection

Each button has an `describe()` method that prints in plain English what the button does and what it renders on the deck.

Each button has an `inspect(what: str)` method that exposes internal values and state. The inspect method takes one parameter `what`  that determines what is displayed when invoked.

These methods can be invoked from the [[Button Activations for Developers#Inspect|Inspect]] button activation.
