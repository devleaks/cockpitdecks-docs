Here is a suggested workflow for web deck design.

# Workflow

Ideally, X-Plane simulator should be running to get live response data.

Get a background image. `PNG` or `JPEG` only.

Place it in

```
<Aircraft>/deckconfig/resources/decks/images/<image-name>.png
```

Start cockpitdecks for that aircraft:

```sh
$ cockpitdecks-cli <Aircraft>
```

Cockpitdecks will start the application server, notice the available background image and Deck Designer will be available.

# Deck Designer

Head to the Deck Designer page at

```
http://<hostname>:7777/designer
```

and select the above image.

Add *interactors* like buttons, encoders, and hardware images. Resize interactors, position them over the background image, and name them.

Double click inside the button to resize it.

Click outside the button to deselect it.

Press Delete key when selected to remove it.

Double on the label to rename it. Press ++enter++ to quit the edit text area.

Save the layout. You can load it later if you wish and continue editing.

We recommand creating a button named `reload` and place it at a convenient location on the image.

Saving the layout will create a new Deck Type, and will add a new deck in the config.yaml file, like any other deck, and add it to the secret.yaml file as well.

```
<Aircraft>/deckconfig/resources/decks/types/<image-name>.json
<Aircraft>/deckconfig/resources/decks/types/<image-name>.yaml
```

Reload the decks, which will provoke the reload of virtual web decks as well.

> [!NOTE] Web Deck Definitions
> Web deck definitions are located either in the Cockpitdecks code or in the aircraft deckconfig folder. On page reloads, web decks definitions located in the code are **not** reloaded. Web deck definitions in the aircraft folder are reloaded.

# Button Designer

Head to the web deck home page.

```
http://<hostname>:7777/
```

Your new deck appears. In the web deck list, select the newly created deck it will open in a new window.

Using the button designer create and test a new button for your new deck selectable at the top.

Select the deck, give a name to a layout (default would be default if none provided), and to the page.

If you followed our advise, there should be a button named `reload`, the label we gave it in the Deck Designer.

Select reload as Activation.

Select icon as Representation. Select icon named "reload.png".

Press render to preview rendering.

Press save to save the layout/page/button.

Reload pages immediately preview the appearance on the new button.

From now on, it is now possible to adjust any of the layout, or button definition, save them, reload and use the button.

When happy with the final deck, I simply comment out the code of the reload button. You will need it later…

```
index: reload
type: reload
icon: reload.png
```

# A Word of Advise

Never ever forget that the goal of these designer tools is currently not to provide you with a final design ready to be used. One day may be. The above designer tools aim at providing you with skeleton files that contain data that is difficult to get or estimate, thereby removing numerous trial and errors attempts.

The goal of the Deck Designer is to supply deck image positions and sizes for all items that need displaying on a web deck.

The goal of the Button Designer is twofold:

1. Help you test button design right away, viewing a button’s appearance without requiring numerous « deck reload page ».
2. Learning the button design options and capabilities without searching through the code.

I hope both tools reach their goal and help you design buttons faster for your decks.

One more thing…

Never ever forget enjoy flying with your home made set up.

If Cockpitdecks crashes or is not responding, just restart it. It will restart, reconnect, and reload necessary data. If failures are persistent, just drop us a mail with a description of the problem and `Cockpitdecks.log` file.

Now just go and take off for new adventures.

# Automation

Using an external file watcher, it is possible to provoke a deck reload each time a file has been saved, using curl(1) to post a request for page reload to Cockpitdecks. There is a handy shell script that does exactly this using nodejs nodemon utility.

```sh
$ nodemon -w aircrafts/*/deckconfig/resources/decks/types -e yaml --exec curl "http://127.0.0.1:7777/reload-decks"
```

# Moving on from there

From there one can:

- Add more buttons, encoders, hardware button.
- Adjust the layout, resize buttons, move them.
- Save the new layout.
- Define and save more button definitions.
- Reload the deck to preview.

When the deck is completed, it is advisable to rename it.

Change name and cross references to name in config, secret, and layout pages.
