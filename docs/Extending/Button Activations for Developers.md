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
Provoke the output of information for all buttons of all pages or all decks.
Mainly for development purpose.
Inspection always starts at the Cockpit level and may or may not crawl down into its constituting parts like decks, layouts, pages, and buttons. Some inspection terminates earlier in the drill down. For example, `threads` inspection stops at the Deck level.

## Attribute
Button Inspect has one attribute
`what`
The value of the `what` attribute determine what is displayed when activated.

### what Attribute Values
`threads`: list Cockpitdecks and deck threads that are currently running.
`datarefs`: list datarefs and values (the page containing the buttons need to be loaded first before it can display values).
`datarefs-listener`: List all datarefs and which buttons (listeners) are using the dataref.
`status`: list internal variables and statuses of each button.
`valid`: check button validity and report invalid status.
`desc`: print a description of what each button does in plain English, both for activation and representation.
`image`: produces an image of each deck, images are saved in the Cockpitdecks home directory and named after the deck.
`longpress`: list [[Button Activation#Longpress|Longpress]] command that should have a couple of additional commands [[X-Plane Interaction#Activation|added to X-Plane through the plugin]].

```Yaml
index: 4
type: inspect
what: desc
```

When button index 4 is pressed, it will display what each button does (description) in plain English on the output or debugging screen or file.