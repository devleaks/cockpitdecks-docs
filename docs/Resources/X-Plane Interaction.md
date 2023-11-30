The interaction with X-Plane is two folds.

# Activation

When a button is activated, Cockpitdecks will either submit a command, or ask to change the value of a writable dataref, or both.
It will also, optionally, execute another `view` command. The purpose of the view command is to modify the focus proposed on the screen consecutively to the first command executed. For example, when the `Status` ECAM button is pressed, the `view` command changes to show the ECAM display.

# Representation
To render an image or an icon on a deck key, Cockpitdecks will use the values of one or more datarefs and/or values internally kept by the button, like the number of times it was pressed.

## X-Plane Dataref Data Source
When Cockpitdecks create a new Button, it will collect all datarefs used by the button. It knows how to find them through the Activation and Representation attributes.
The Page where the button is located will collect all datarefs of all buttons on that Page.
When a Page is displayed on a deck, Cockpitdecks will ask X-Plane to periodically send the values of all datarefs used on that page.
Cockpitdecks will periodically receive all dataref values, and when the value of a dataref has changed, all buttons that use the dataref will be notified of the change. The button can then handle the change, may be re-compute its value, and change its appearance accordingly.
When a Page is unloaded from a deck, Cockpitdecks ask X-Plane to stop sending datarefs values for the datarefs the page uses. X-Plane may, however, continue to send the value of datarefs for other decks connected to it. A button may therefore receive notifications of dataref value changes, even if it is not currently displayed. Buttons will handle the change of value, including computations, but it will not render since it is not currently displayed on a key.

## Non Dataref Data Sources
Alternatively, a button can also use values coming from other sources to render. Currently, buttons are able to use internal state values such as the number of times a button was pressed, or the number of times an encoder was turned clockwise.

## Special Buttons with External Values
The weather button display the latest, current Metar of an airport. It is automagically updated every 30 minutes. It actually uses two datarefs (latitude and longitude of aircraft) to guess what is the closest Metar station. It is updated if the closest station changes, or every 30 minutes. (The refresh time is a parameter.)
The Metar for the closest station is fetched from a remote server, hence the isolation of these types of buttons (that fetch values outside of X-Plane eco-system) from other, more common buttons.
The TAF works in a similar way.