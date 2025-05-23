Weather representations have been split in two components:

1. Weather Data
2. Representation

Weather Data is itself a combination of two components:

1. A Location, which is either fixed, or moving (aircraft position).
2. The actual weather information in a coded form (METAR, TAF, SPECI…)

Location is optionally tracked with aircraft position, or dynamically defined through a (string) dataref that defines the station to use.

# Weather Representation

Weather representation is responsible for creating and updating an image representation of the weather.

## Textual Representation

METAR and TAF.

For METAR, short, succinct textual representation of the weather. The background image sof the weather METAR or SPECI is a weather icon that depicts the actual weather.

For TAF with multiple pages of forecast, each press of the icon cycles through the pages. Each forecast is a textual representation if its coded transmission.

## Graphical Representation: « Station Plot »

METAR only.

Graphical representation is a [Standard Model Plot](https://en.wikipedia.org/wiki/Station_model).

There is an « official », standard Station Plot in either metric or imperial units that reproduces the above data.

There also is a fancier, colorful modern representation with non standard information but highly practical. For example, ceiling information usually absent from Standard Model Plot. Fancier Standard Plot tries to ally conventions and iconic modern user interface elements.

# Weather Data

The Weather Data entity is responsible for collecting and maintaining coded weather information.

The collection occurs at regular time interval or speed (time flows at normal speed, not accelerated or slowed down.)

The following two events are handled « on demand » by the weather data service:

1. Change of location.
2. Change of date/time for historical METAR.

Change to the above data may result in weather data changes, which are transmitted to weather data listeners through the `weather_changed()` function.

Weather Data exposes information in any format that is suitable for its reprensetation. However, in the context of aeronautics, Weather Data exposes information in a standard, weather coded representation like METAR.

## Data Sources

As today, the following Weather Data source are available:

1. Real life current coded weather as METAR or TAF.
2. X-Plane « Real Weather » (METAR only).
3. Ogimet « historical » (past) METAR and TAF, if available.

# Attributes

Configuration attributes.

Global: animation frequency (`speed`)

## Location

Moving or fixed.

If moving: How often it is checked (because it is expensive to check).

Check = moved a certain distance, then station has changed.

## Weather

If station changed, reload.

If expired: reload. Expired since last load.

Real weather: aircraft or region. Always changes with aircraft position. Default is region.

To do: Real weather from METAR in txt file in xplane folders.

Live weather: fixed location or moving with aircraft.

Fixed location: either supplied or guesseD from MCDU dep/arr (Toliss specific).

## Presentation

Colors and sizes.

Units.

Fancy station plot with Colors and ceiling information.

## Fancy (« Practical ») Station Plot

Standard Staton Plot relies on numerous conventions and it takes a few readings before some values are naturally interpreted. In addition, throw a mix of metric (pressure) and imperial (flight levels, distance in miles) units, and the little circle of numeric values becomes tricky to read.

Station Plot were originally targeted at map overlay, sometimes in high densities. Fewer number the better.

This lead to the design of an equivalent circle with similar data at the same location around the circle, with the same meaning but with different units, coding, and conventions. In addition, units are often added through a simple, yet unambiguous character or sign.

Color is also added as a supplemental information, from green, insignificant values, to red, warning ones. Each value is coded independently.

### Examples

Atmospheric pressure is shown explicitly like 983, 1013 or 29.92.

Visibility is show in meter below 1000m, and in kilometer above. 60m, 600m, 6k, 12k, 30k.

All values are shown in the same unit system, metric by default. All metric or all imperial.

Graphical symbols remain unchanged, but sometimes to meaning of the symbol is added under the symbol with a small text.

This undoubtedly creates a iconic image with more signs, probably unsuitable for map overlay, but easier to read and understand for occasional flight simmers.
