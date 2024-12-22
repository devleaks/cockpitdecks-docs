Cockpitdecks uses two types of value that can come from different sources:

1. **Data** are simple *scalar* typed values (mainly string, float or integer). They can either come from the simulator software or Cockpitdecks internal values.
2. **Value** are complex scalar typed values that depends on one or more **Data**. If the Value depends on more than one Data, a **Formula** is used to combined all Data together and ultimately provide a single final value.

Values are used by buttons and observables. Values are used when there is a need to combine and modify raw values as provided by the simulator or by Cockpitdecks.

Values are notified when one of its underlying Data has changed. This allows the Value to compute its new value. the Value in turn notifies the Button or the Observable that created it of its change. The Button or Observable can adjust as needed.

Changes of Data value enter Cockpitdecks through Events. The SimulatorEvent (or one of its sub-class) is used to report the change of a value in the simulator software. The CockpitEvent (or one of its sub-class) to report change of value in the Cockpit.
