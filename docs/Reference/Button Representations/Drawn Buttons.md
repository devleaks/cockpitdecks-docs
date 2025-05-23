A few set of special buttons are dynamically drawn based on attributes.

Attributes are numerous to allow for design flexibility, however default values will always provide a usable representation.

The idea of drawn buttons came after the flexibility discovered when designing [[Annunciator|Annunciators]]. All annunciators are drawn buttons: Texts, frames, basic icons…

# Data

A DataButton is a particular case of a display only button.

![[data.png]]

A DataButton displays four informations:

1. A single letter or icon from a character font, (we can use icon fonts,)
2. A value, and optionally the trend of the value (rising, decreasing, statuquo)
3. A unit short text (static)
4. An optional percentage bar of the value relative to a 100% value
5. A small text string (typically ~20 characters maximum) called the bottom line.

```yaml
  - index: 4
    name: Fuel Level
    type: none
    label: Fuel
    label-size: 10
    label-position: ct
    data:
      icon-name: "gas-pump"
      data: 75.4256
      data-format: "{:02.0f}"
      data-font: DIN Condensed Light
      data-size: 24
      data-unit: "%"
      data-progress: 100
      data-trend: 0
      dataref-rpn: ${sim/aircraft/fuel_level} 10 *
      bottomline: Go Faster
```

Data button representation aims at providing a dashboard-like single value highlighted in a deck key image, very much like common web dashboards.

![[dashboard.png]]

# Decor

Decor representation displays simple connected lines to populate unused icons. They can be used to display visual helper lines that connect annunciators (bleed air, hydraulics, etc.) The idea behind Decor icons is to provide a quick alternative to blank icons when filling large decks with numerous keys.

Decor icon are governed by two parameters `type` and `code`. The type is a category of drawings. The code determine which drawing will be made in that category.

## Lines

`type: line`

Decor icons of type `line` display a single horizontal or vertical line, and corner angles. The `code` determine which line get drawn.

![[decor.png]]

```yaml
type: line
code: H
```

![[H.png]]

## Segments

`type: segment`

Decor icons of type `segment` display segments that are present in the `code` attribute.

![[segments.png]]

For example:

```yaml
type: segment
code: BGNSIL0123
```

will light (turn on) segments B, G, N... 3, which correspond will result in a drawing like the one proposed by the `H` code in type `line` above.

![[I0123LBGNS.png]]

## Common Decor Attributes

### Line width

`width: 10`

Width of the line.

### Color

`color: red`

Color of the line.

# Aircraft

The aircraft representation displays the name of the aircraft (ICAO type designator) located in the dataref: `sim/aircraft/view/acf_ICAO`. All 4 (or more) characters are fetched and displayed in the icon. (We currently limit fetching the first four characters only, which should be sufficient for ICAO aircraft code designator.)

The representation attribute is

```yaml
    aircraft:
      text-font: B612-Bold
      text-size: 32
```

If a `set-dataref` is present, the aircraft representation increases the value of that dataref by one each time the aircraft name changes in the `sim/aircraft/view/acf_ICAO`. This can be used by other button or by Cockpitdecks itself to be notified of an aircraft or aircraft model change.

# Weather

(Please refer to the dedicated [[Reference/Button Representations/Weather|Weather Representation]] page.)

## METAR

The WeatherButton is a special data button that displays METAR information of the station closest to the aircraft in a small, iconic representation.

```yaml
  - index: 8
    name: METAR
    type: weather
    station: OTHH
```

![[weather.png]]

## TAF

(To do. Cycle through forecast each time button is pressed.)

## Current X-Plane Weather

### Region

### Aircraft
