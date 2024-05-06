Cockpitdecks offers different types of Weather Representation, mainly on icons.

These Representations, together with accompanying custom [[Cockpitdecks Internals#Dataref Collector|Activations]], have led to development of highly customized activations and representations to exhibit the potentials of Cockpitdecks.

While some Representations remain highly generic, like graphical representation of METAR at the closest station, some others are highly specific like those that represent X-Plane simulated weather.

# X-Plane Weather

X-Plane weather is flexible and proposed with several variant. We designed a few set of X-Plane specific button representation to present these data sets.

- Weather from external source (METAR, TAF)
- Weather from X-Plane (from global items like METAR)
- Weather informations from X-Plane (from clouds and wind *layers*)

In addition, X-Plane weather can be regional or global.

# Live Weather

```yaml
live-weather
	station: LFBO
	update: 30  # minutes
```

Reports the current live weather for the closest Metar location. (Closest to the current aircraft location.)

# Real Weather

```yaml
real-weather
	mode: region
```

Provides the Real weather currently installed into X-Plane in a short, Metar-like summary.

This is done by fetching a limited set of values from the weather-related datarefs (temperature, pressure, wind, weather conditions...)

There are two *modes*: `aircraft` and `region`.

# XP Weather

```yaml
xp-weather
	mode: region
```

Provides a detailed weather report from all weather-related datarefs.

There are two *modes*: `aircraft` and `region`.

This representation fetches all weather-related datarefs from the simulator (wind layers, cloud layers, weather conditions, more than 100 datarefs) for a location or region and attempt to automagically generate a METAR from the collected data.

To collect and monitor such an amount of datarefs, Cockpitdecks designed [[Button Value#Dataref Sets|Dataref Sets]].
