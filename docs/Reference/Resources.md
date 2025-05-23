# Resources

## Fonts

## Icons

## Sounds

# Custom Decks

## Structure

### Deck Type

### Web Deck Background Image

# Observables

[[Observables]]

# Additional Configuration Attributes

`config.yaml`

## Default Values

## Named Colors

## Theme

Rendering themes is a features that allows a Cockpitdecks developer to provide different sets of default values for colors, fonts, and textures.

### Theme Declaration

```
cockpit-theme: night
```

Theme default value is an empty string (no theme).

If a theme is used, its name is prepended to default attribute name when they are looked up:

```
	night-default-label-color: purple
```

If no such attribute is found, the `default-label-color` attribute name is looked up, without the `theme-` prepended.

### Theme Usage

When a theme is defined, its name is prepended to *some* default values.

Default values affected by themes are:

- `cockpit-color` and `cockpit-texture`
- All attributes that end with `color`. Example `default-color`.
- All attributes that end with `texture`. Example `annunciator-bg-texture`.
- All attributes that end with or contain the word `font`. Example `label-font`.

```
theme: light
# Alternate theme is 'dark'
default-label-color: white
dark-default-label-color: coral
```

It is advisable to define a light/day theme and a dark/night theme. There is an activation to change the theme. It also is possible to change the theme with an Observable. For example, it is possible to collect the simulation date, time, and location, and determine if it is a day-light or night time and set the theme accordingly.
