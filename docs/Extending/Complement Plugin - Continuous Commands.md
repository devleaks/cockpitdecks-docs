To circumvent limitations of UDP, a plugin is necessary to execute commands that require to be activated for a long time (like long push commands).

For each long push command, the plugin will create a pair of commands that will be issued at the beginning of the command, and at the end.

The plugin scans deckconfig folder for button definitions that contain `longpress`activation. It then creates a pair of commands for the command specified in the command attribute.

```yaml hi_lines="9"
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
