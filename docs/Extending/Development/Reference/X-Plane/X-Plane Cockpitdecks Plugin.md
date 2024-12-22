### Why an Additional Plugin?

X-Plane UDP has some shortcomings that prevent some operations with decks.

To circumvent this, Cockpitdecks provides a small python plugin called the Cockpitdecks Helper plugin, that need to be installed into X-Plane. The Cockpitdecks Helper plugin will execute some instructions on behalf of the Cockpitdecks application. Cockpitdecks Helper plugin just need to be installed and will provide its services to Cockpitdecks.

If not installed, some of the commands inside Cockpitdecks will work properly.

Cockpitdecks Helper plugin is an _in-process_ plugin, running inside X-Plane, while Cockpitdecks is an _out-of-process_ executable running independently of X-Plane.

Here are the functions the Cockpitdecks Plugin offers to complement X-Plane UDP access.

#### Long command execution

Some commands cannot be executed directly through UDP. For exemples, commands that have a *start* and an *end* cannot be started or ended though UDP. It is an X-Plane UDP limitation.

To execute long press commands, the **Cockpitdecks Helper** plugin needs to be installed in XPPython3 `PythonPlugins` folder.

#### String Datarefs

X-Plane UDP only allows to fetch dataref values one by one. Retrieving a string is a tedious process because each character has to be fetched individually.

To collect string-typed datarefs, the **Cockpitdecks Helper** plugin needs to be installed in XPPython3 `PythonPlugins` folder. It collects the entire string and then broadcasts it as a whole string, not individual characters.

See [[String Datarefs]] for details about this.

---

To circumvent limitations of UDP, a plugin is necessary to execute commands that require to be activated for a long time (like long push commands).

For each long push command, the plugin will create a pair of commands that will be issued at the beginning of the command, and at the end.

The plugin scans deckconfig folder for button definitions that contain `longpress`activation. It then creates a pair of commands for the command specified in the command attribute.

```yaml hl_lines="9"
  - index: 27
    type: long-press
    label: "APU\nTEST"
    push-switch:
      button-size: 120
      button-fill-color: black
      button-stroke-width: 0
      witness-size: 0
    command: AirbusFBW/FireTestAPU
    formula: 1
```

The above button definition will create the pair of commands

`AirbusFBW/FireTestAPU/begin`

and

`AirbusFBW/FireTestAPU/end`

`AirbusFBW/FireTestAPU/begin` will be called when the button is pressed, `AirbusFBW/FireTestAPU/end` when released.
