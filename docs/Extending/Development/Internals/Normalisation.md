Items here are more in a wish list rather than completed development

# Button definition

Try: button meta (name, etc.) + global parameters + activation attributes + representation attribute name are all at minimal indent level.

Representation is a next indentation.

# Formalize

Checl: set-dataref copies the **button VALUE** to the dataref

# Value

## Single value:

Only two possibility:

1. A single dataref (then we return the value)
2. A formula (then we return the value of the formula or None if error)

## Multiple values: (check that they are always all *dict* and not list)

1. Multiple datarefs (multi-datarefs)
2. All values of individual annunciator parts
3. All values of button states

# Introspection

Save stats on deck, page, buttons in internal datarefs

Save stats on connection, speed of collection, speed of processing, etc.

(Try something "à la Oracle": wait time on what?)

## Cockpits

number of plane change

number of definition reloads (deck reloads, page reload)

time since last start/reload/longest

## Deck

number of page load

## Page

number of load

number of button activations

## Button

Number of activation

Number of render

## datarefs

number of update

If redis? save in redis?!
