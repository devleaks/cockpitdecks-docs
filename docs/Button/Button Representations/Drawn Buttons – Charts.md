Cockpitdecks allows to represent a dataref value on a simple chart, or rather, a simple [Sparkline](https://en.wikipedia.org/wiki/Sparkline). Up to three Sparklines can be combined on a single chart icon.

# Sparkline

A Sparkline is a chart element that plot a single variable over time.

A Sparkline keeps a number of points to be plotted.

The maximum number of point that can be plotted is either directly supplied, or determined by the time width of the chart. (Points older that the oldest point on the chart are discarded.)

## Sparkline Representation

The list of time-series data can be plotted as

- Points: A marker at each position
- A Curve: A simple line
- An histogram: A set of vertical bars

![[chart-anatomy.png]]

# Data Acquisition

A Sparkline is based on a value like a button value.

The value is either fetched at regular interval or updated each time it changes.

Therefore, a chart will be updated either at regular interval, if at least one of the data is fetched at regular internal, or whenever the dataref value has changed.

There is a maximum update/refresh rate fixed at 4 Hz (1/4 of a second.)

# Chart Width

The horizontal axis of charts is always the time.

The maximum time that is displayed on the total width of the chart can be determined either by directly supplying a maximum time width, or by supplying a number of points to keep, spaced at a regular interval.

# Example

![[sample-3charts.png]]

# Performance Notes

Charts a little icons that can involve a lot of CPU cycles, from dataref collection, change detection, graph data update and then graph update.

We limit the update of graphs to 4 Hz maximum (that is 4 times per seconds).

There is a limit on the number of Sparklines (graphs) in a single icon: 3.

There is also a limit on the total number of Sparklines that can be use at one point in time: 10, all displayed decks combined.

# Examples of Use

- Fuel per tank and fuel flow per engine.
- Autopilot heading vs. actual aircraft heading.

##### Internal uses

- Performance data like speed to UDP packet acquisition
- Speed of deck rendering
- and so on.
