Rendering themes is a experimental features that allows a Cockpitdecks developer to provide different sets of default values.

# Theme Declaration

```
cockpit-theme: night
```

Theme default value is an empty string (no theme).

# Theme Usage

When a theme is defined, its name is prepended to *some* default values.
 
Default values affected by themes are:

- `cockpit-color` and `cockpit-texture`
- All attributes that end with `color`. Example `default-color`.
- All attributes that end with `texture`. Example `annunciator-bg-texture`.
- All attributes that contain the word `font`. Example `label-font`.

## Themed Default Values
