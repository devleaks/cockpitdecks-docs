Cockpitdecks offers different types of Weather Representation, mainly on icons.

These Representations have led to development of highly customized activations and representations to exhibit the potentials of Cockpitdecks.

While some Representations remain highly generic, like graphical representation of METAR at the closest station, some others are highly specific like those that represent X-Plane simulated weather.

There are two sources of information for weather:

1. External, real weather collected from aviation sources.
2. X-Plane, using the weather as it currently is available in the simulation.

# Real Life Live Weather (now)

The WeatherMetarIcon provides the *real* Metar currently in use at a location in the simulator.

If the location is provided, it is used and never updated.

If no location is provided, the *current aircraft location is used*. If the aircraft is cruising, the location is updated approximatively every 5 minutes, or when the closest Metar source changes.

If no aircraft location is available, a default location is used (currently EBBR, Brussels Airport, Belgium).

```yaml hl_lines="1"
weather-metar:
	station: LFBO
	update: 30  # minutes
```

Reports the current real life weather for the closest Metar location. (Closest to the current aircraft location.)

If the location is not changed, the Metar updates every `update` minutes.

> [!NOTE] Metar is from external source
> The WeatherMetarIcon reflects the real Metar at the time of collection. It does not necessarily reflect the weather as simulated in X-Plane.

> [!WARNING] X-Plane Weather
> X-Plane Weather is currently heavily modified by Laminar. The following representation do not work reliably.

# X-Plane Weather

X-Plane weather is flexible and proposed with several variant. We designed a few set of X-Plane specific button representation to present these data sets.

- Weather from X-Plane (from global items like METAR)
- Weather informations from X-Plane (from clouds and wind *layers*)

In addition, X-Plane weather can be local or regional.

# X-Plane Weather Summary

```yaml hl_lines="1"
xp-weather-summary
	mode: region
```

Provides the Real weather currently installed into X-Plane in a short, Metar-like summary.

This is done by fetching a limited set of values from the weather-related datarefs (temperature, pressure, wind, weather conditions...:

1. pressure: sealevel_pressure_pas,
2. temperature: sealevel_temperature_c,
3. dew point: dewpoint_deg_c,
4. visibility: visibility_reported_sm,
5. wind direction: wind_direction_degt,
6. wind speed: wind_speed_msc,

There are two *modes*: `aircraft` and `region`.

# X-Plane Real Weather

```yaml
xp-real-weather
	mode: region
```

Provides a detailed weather report from all weather-related datarefs.

There are two *modes*: `aircraft` and `region`.

This representation fetches all weather-related datarefs from the simulator (wind layers, cloud layers, weather conditions, more than 100 datarefs) for a location or region and attempt to automagically generate a METAR from the collected data.

To collect and monitor such an amount of datarefs, Cockpitdecks uses the X-Plane Web API, only available from release 12.1.1 onwards.
