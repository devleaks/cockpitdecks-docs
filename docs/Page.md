A *Page* is a collection of button definitions that will be installed and used on the deck at a given moment.

A deck displays one Page of buttons and allow end-users to press those buttons to trigger actions. Actions to be performed when a button is manipulated are defined in the button definition, together with the appearance of the button on the deck. Actions are constrained by the type of push button or encoder. A push button cannot be rotated. The appearance of the button is also constrained by the feedback ability of the deck: Image icon, LED, colored LED, or, sometimes, no display at all.

## Attributes

A Page contains the following attributes:

```yaml
name: Index
buttons:
  - index: knobTL
    name: FCU Airspeed
    type: knob
    commands:
      - sim/autopilot/airspeed_down
      - sim/autopilot/airspeed_up
    options: dot, nostate
includes: views
fill-empty-keys: true
```

### name

Name of the Page. Superceedes the file name. Case sensitive.

If no name is given, the name of the page is the page file name (without the `.Yaml ` extension.)

For a given layout, all pages must have different names, otherwise an error is reported, and the page with the same name is ignored.

### buttons

**Mandatory**.
A list of button definitions. Please refer to the [[Button]] Section for details about button definitions.

### includes

Includes is a list of other pages to include in this page.

Included pages are first merged inside the main page, and then submitted for display and use.

### default-*

Default values to be used at the Page-level for display.

### fill-empty-keys

Whether to fill unused keys with a default button definition as a placeholder.

The default value of some attributes (like font, colors, and sizes) is fetched at the Layout level if they are not specified at the Page level.

# Buttons

The `buttons` attributes contains all [[Button|button definitions]].

# Default Page

If no Page is found in the Layout folder, Cockpit deck will create a default page which consists of a logo displayed on the deck (if the deck is capable of displaying images...).

The first available button will be programmed to toggle the X-Plane Map On or Off.
