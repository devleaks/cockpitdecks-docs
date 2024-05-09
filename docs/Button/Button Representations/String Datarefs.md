Cockpitdecks extensively uses X-Plane UDP for communicating with the simulator. X-Plane UDP suffers from shortcomings, one being its inability to collect several values of an array at once. Values need to be fetched one a a time, which can be not only tedious but also have an impact on performance by requesting numerous single values.

We reasonably established a empiric limit around 50 to 80 individual datarefs values before it has a significant impact not only on X-Plane but also on Cockpitdecks.

To circumvent this limitation, Cockpitdecks provides a simple mechanism to fetch entire arrays at once. However, this mechanism has voluntarily be limited to arrays of bytes, i.e. character strings. (But the mechanism could be used to fetch arrays of numeric values as well.)

The mechanism relies on a small XPPython3 plugin that collects values of datarefs of type string (array of bytes) and broadcast the values on a UDP port, very much like X-Plane UDP does for float values.

# Button Declaration and Use

To use datarefs of type string, a button has to tell Cockpitdecks it want to use such a dataref.

```yaml hl_lines="5"
index: 1
name: AIRCRAFT
label: Aircraft
type: none
string-datarefs: ["sim/aircraft/view/acf_ICAO", "sim/aircraft/view/acf_tailnum"]
text: "${sim/aircraft/view/acf_ICAO}\n${sim/aircraft/view/acf_tailnum}"
text-font: B612-Bold
text-size: 18
```

Given the particular nature and treatment of string datarefs, they have to be explicitely defined with a specific `string-datarefs` attribute which expects a list of datarefs of type string as a value.

These datarefs will be collected by Cockpitdecks’ custom mechanism and can then be used like any other datarefs.

# XPPython3 Plugin

The XPPython3 plugin proceeds as follow. For the aircraft currently being used by X-Plane, it reads the Cockpitdecks configuration in the `deckconfig` folder, if any.

If it finds one, it inspect decks, their layouts, pages in these layouts, and ultimately buttons being used on each page, scan for `string-datarefs`  attributes in button definitions, and collects string datarefs that are used. It then collects the values of those datarefs and publish them on a dedicated UDP port, all at once at a regular interval. No more, no less.

# Cockpitdecks Internal Mechanism

Cockpitdecks treats string datarefs a little bit differently. When encountered in a button definition, they are created first with their particular type (string). Other datarefs are created after.

Also, string dataref values are not collected throught X-Plane UDP but by another similar mechanism running on another UDP port, collecting values sent by the XPPython3 plugin.

A part from those internal differences, string datarefs behave like any other datarefs, notifying the buttons that rely on them when their value is updated.
