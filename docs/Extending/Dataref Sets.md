> [!WARNING] The Dataref Set Collector does not work properly
> It is a difficult piece of software. "It works", but has a significant impact on other Cockpitdecks loops and threads. It is advisable to avoid using it, especially on performance limited computers.

# Dataref Sets

A 32 button page can quickly request more than 40 datarefs from the simulator. The simulator takes a few precious cpu cycles to pack and send datarefs. We experimentally found a limit around one hundred datarefs. The fewer requests, the better. Unless otherwise specified, Cockpitdecks request a dataref once per second. Datarefs are sent in UDP packets of about 14 values. It takes on average 2 or 3 UDP packets to get all dataref values.

To limit to number of requested datarefs at a point in time, and to allow for almost unlimited datarefs requests, Cockpitdecks has the concept of *Dataref Set*.

A Dataref Set is a limited number of datarefs that are requested from the simulator together.

The **Dataref Set Collector** is responsible for scheduling the collection of multiple dataref sets, one at a time.

Each dataref in a set is monitored, value changes are registered, a Dataref Set is said to be *completed* when all dataref values have been received at least once.

A button can have a `dataref-sets` attribute, it is a set of individual *Dataref Set*s. The attributes of a Dataref Set are described in the following Section. A button will receive completion events for each individual set, and anither « total » completion event when all sets are completed.

## Dataref Collection Attributes

### Name

Collection name. Must be unique for Cockpitdecks.

Attempt to register a set with an existing name will fail to register the second set.

### Datarefs

List of datarefs in set.

There is a maximum value of datarefs in a set (20). If more datarefs are requested, a warning is sent and additional datarefs are ignored.

### Time to Live

How long, after all datarefs have been collected at least once (set is *completed*) does the Collector schedule the collection again for update. The set will be completed again after all datarefs have been updated *after* the collection has been requested for update.

### Array

The `array` attribute is an integer value. If present, it tells that each datarefs in the `datarefs` attribute is an array of values of size `array`. All datarefs in the set must have the same array length. As a result, this attribute will lead to the creation of `array` sets, one fo each element of the array of dataref values.

Typical arrays of values that can be requested as such are weather data like clouds or wind layers:

```
set-dataref: data:all-cloud-layers-completed
dataref-collections:
  -
    name: cloud-layers
    datarefs:
      - sim/weather/aircraft/base
      - sim/weather/aircraft/tops
      - sim/weather/aircraft/cloud_type
      - sim/weather/aircraft/coverage
    array: 3
    set-dataref: data:cloud-layer-completed
  -       
```

The above example will create 3 equal sets of 1 datarefs and is equivalent to the following:

Please note how sets are named after the root name above.

```
dataref-collections:
  -
    name: cloud-layer#0
    datarefs:
      - sim/weather/aircraft/cloud_type[0]
    set-dataref: data:cloud-layer-completed[0]
  -
    name: cloud-layer#1
    datarefs:
      - sim/weather/aircraft/cloud_type[1]
    set-dataref: data:cloud-layer-completed[1]
  -
    name: cloud-layer#2
    datarefs:
      - sim/weather/aircraft/cloud_type[2]
    set-dataref: data:cloud-layer-completed[2]
  -
```

### set-dataref

A dataref that is set to an incremental value each time the *set* is completed.

In the above example, `data:cloud-layer-completed[2]` local dataref will be set to an incremented value each time the set of four datarefs is completed, while the value of local dataref `data:all-cloud-layers-completed` will be set to an incremented value when all three sets are completed.

> If a Dataref Collection is too large, it simply can be split into smaller Collections.
> A dataref can be part of multiple sets.

## Dataref Sets Dataref Values

Datarefs in sets are not available through the Page. They must directly be accessed through the set in the Collector. The Collector is part of the [[Simulator]].

#### Access in Page

```python
value = page.get_dataref_value(dataref_name)
```

#### Access in Dataref Sets

```python
value = simulator.collector.collections[collection_name].datarefs[dataref_name]
```

## Dataref Sets Collector

The Dataref Sets Collector fetches sets of datarefs one at a time.

It is started by the Simulator.

If no set needs updating, the Collector does nothing.

If a set is loaded for update, the Collector regularly monitor its progress. When all datarefs in the set have been updated (the set is said to be *completed*), the Collector unloads the set from update and notifies the set's owner of completion. If some dataref in the set do not get updated for a while (timeout), the Collector temporary unloads the set and schedule another set for collection. Sets are scheduled randomly with a probability that increases over time to prevent starvation (very much like UNIX nice(1)).

The Dataref Sets Collector is awaken every minute to check whether sets need updating.
