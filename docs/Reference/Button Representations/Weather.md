Cockpitdecks offers different types of Weather Representation, mainly on icons.

These Representations have led to development of highly customized activations and representations to exhibit the potentials of Cockpitdecks.

While some Representations remain highly generic, like graphical representation of METAR at the closest station, some others are highly specific like those that represent X-Plane simulated weather.

There are two sources of information for weather:

1. External, real weather collected from aviation sources.
2. X-Plane, using the weather as it currently is available in the simulation (called real weather inside X-Plane eco system.)
![[weather-representation.png]]

# Real Life Weather (now)

The WeatherMetarIcon provides the *real life* Metar currently in use at a location in the simulator.

If the location is provided through an attribute, it is used and never updated. If no location is provided, the *current aircraft location is used*. If the aircraft is cruising, the location is updated approximatively every 5 minutes, or when the closest Metar source changes.

If no aircraft location is available, a default location is used (currently EBBR, Brussels Airport, Belgium).

```yaml hl_lines="1"
weather-metar:
	station: LFBO
	update: 30  # minutes
```

Reports the current real life weather at LFBO. If the location is not changed, the Metar updates every `update` minutes.

> [!NOTE] Metar is from external source
> The WeatherMetarIcon reflects the real Metar at the time of collection. It does not necessarily reflect the weather as simulated in X-Plane.
> As a current limitation, only current METAR or TAF report is shown at the time the application runs. Historical data may be offered at a later stage.

This Representation is provided as an extension in the cockpitdecks_wm package (wm=weather METAR).

## Terminal Area Forecast

```
weather-taf:
	station: EDDM
	update: 30  # minutes
```

Similarly to METAR, TAF is fetched and shown on an icon as a textual form of the forecast.

A TAF usually consists of several forecasts that are presented in turn on "pages" of forecast. Each press of the button changes the page.

## (Ground) Weather Station Plot

[Station Model](https://en.wikipedia.org/wiki/Station_model) is a popular representation of the main weather elements: Weather, wind speed and direction, temperature, pressure, clouds, visibility, etc.

```
weather-station-plot:
	station: LFBO
	update: 30  # minutes
```

The plot model is a graphical representation of the METAR. The same information is used for text and plot.

## Examples

![[weather-ext.png|300]]

METAR, TAF and two «Station Model» plots of weather. Left most icons (EBBR) uses the same METAR, only presentation differs.

## Weather Updates

If possible, METAR and TAF for a station are temporarily archived and used to present evolution, past and forecast information, very much like real METAR web site.

METAR and TAF are updated every 30 minutes on average.

### Location Update

If the display of the weather is not fixed to a location, the position of the aircraft in X-Plane is used, although, the weather that is fetched is the real weather at the time of the the day (not the simulated date/time). In this case, the nearest weather station is used for the report. Station position changes if the aircraft moved more than a minimal distance, typically 50 kilometers.

# X-Plane « Real Weather »

```yaml
xp-real-weather
	mode: region
```

Provides a detailed weather report from all weather-related datarefs.

There are two *modes*: `aircraft` and `region`.

This representation fetches all weather-related datarefs from the simulator (wind layers, cloud layers, weather conditions, more than 100 datarefs) for a location or region and attempt to automagically generate a METAR from the collected data.

To collect and monitor such an amount of datarefs, Cockpitdecks uses the X-Plane Web API, only available from release 12.1.1 onwards.

> [!WARNING] X-Plane Weather
> X-Plane Weather is currently heavily modified by Laminar. The following representation may not work reliably and may need revisions.

This Representation is provided as an extension in the cockpitdecks_xp package (xp=X-Plane, since it is specific to weather as provided by X-Plane).

# X-Plane Weather Station Update

When using X-Plane flight simulator, a special Observable regularly monitors the position of the aircraft and deduces the closest weather station. The closest weather station ICAO code is stored in the internal variable named `weather-station`. By subscribing (listening) to that variable changes, a representation can adjust its weather information for the local station.
