Cockpitdecks manipulates values that can come from different sources:

1. **Data** are simple *scalar* typed values (mainly string, float or integer). They can either come from the simulator software or Cockpitdecks internal values. They are scalar variables.
2. **Value** are complex scalar typed values that combines on one or more **Data** in a Formula expression which ultimately provide a single final value.

Values are used by buttons and observables. Values are used when there is a need to combine and modify *raw* values as provided by the simulator or by Cockpitdecks into another meaningful value through simple mathematical operations or roundings.

Data and values have listeners. Listeners are other objects inside Cockpitdecks that are using the data or value. When the data or value changes, each listener is notified of the change and can act accordingly.

For example, Values are notified when one of its underlying Data has changed. This allows the Value to compute its new value. the Value in turn notifies the Button or the Observable that relies on it of its change. The Button or Observable can then adjust as needed.

Changes of Data value enter Cockpitdecks through Data Events. The SimulatorDataEvent (or one of its sub-class) is used to report the change of a value in the simulator software. The CockpitDataEvent (or one of its sub-class) to report change of value in the Cockpit.
