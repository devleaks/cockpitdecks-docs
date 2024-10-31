Rendering themes is a experimental features that allows a Cockpitdecks developer to provide different sets of default values.

# Theme Declaration

```
cockpit-theme: night
```

Theme default value is an empty string (no theme).

If a theme is used, its name is prepended to attribute name:

```
	theme-default-label-color: purple
```

If no such attribute is found, the `default-label-color` attribute name is looked up (without the `theme-` prepended.)

# Theme Usage

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
