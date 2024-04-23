
To circumvent limitations of UDP dataref fetch, Cockpitdecks is bundled with a plugin that reads "string"-typed datarefs efficiently and post them on a UDP port, as a single decoded string value, very much like other dataref values sent by the simulator.

The String Dataref Poster is a XPPython3 plugin that reads the `string-datarefs`attribute in the main Cockpitdecks configuration file. The `string-datar` attributes is a list of datarefs that are supposed to be of type "array of bytes", each byte being assumed to be a character ASCII code.

```yaml
string-datarefs:
  - AirbusFBW/FMA1w
...
  - AirbusFBW/FMA3a
use-default-string-datarefs: True
```

The String Dataref Poster will ask the plugin to post the value of each of those datarefs at regular intervals, typically every 5 seconds.
