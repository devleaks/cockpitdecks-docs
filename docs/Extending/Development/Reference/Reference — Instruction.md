An Instruction is an action that will be performed when required through interaction with the deck or Cockpitdecks.

# Instruction Types

## Internal Instruction

Internal Instruction are instruction used by Cockpitdecks for its internal control. Examples of such instructions are:

- Load a new page on a deck
- Stop Cockpitdecks
- Change theme
- ...
or for developers:
- Reload all decks
- Inspect Cockpitdecks components

Internal instruction name starts with `cockpitdecks-` followed by the instruction itself.

```
    command: cockpitdecks-aircraft
```

is an internal instruction that is executed when Cockpitdecks notices a new aircraft has been loaded in the simulation.

> [!NOTE] Internal Instruction name
> The name of an internal instruction starts with `cockpitdecks-`. It is a convention,and it is how internal instructions are detected and treated separately.

## Simulator Instruction

A Simulator Instruction is an instruction sent to the simulator to perform an action there. Example of such instructions are:

- Change a view
- Trigger a command in the Cockpit

```
    command: toliss_airbus/dispcommands/CaptWptPushButton
```

Simulator instruction has no prefix.

# Instruction Attribute

For historical reason, the attribute to specify an instruction is `command`.

## Simple Instruction

In its simple form, a command is just the name of the instruction

```
     command: SRS/X-Camera/Select_View_ID_1
```

## Instruction Block

Another form of instruction is the instruction block. The instruction block is an instruction with two additional attirbutes

**Condition**: A condition to satisfy before the instruction is executed. The condition is evaluated first, each time the instruction is requested to run. If the result of the condition is True (not zero), the instruction is executed.

**Delay**: The delay is the time to wait from the request to run to the actual moment of execution of the instruction.

```
      - command: AirbusFBW/PopUpSD
        condition: ${AirbusFBW/PopUpStateArray[7]} not
        delay: 2
```

## Macro Instruction

A Macro Instruction is a set of one or more commands that are executed one after the other.

Each command can be specified either a simple instruction or an instruction block

```
    command:
      - command: AirbusFBW/ECP/SelectElecACPage
      - command: AirbusFBW/PopUpSD
        condition: ${AirbusFBW/PopUpStateArray[7]} not
        delay: 2
      - command: AirbusFBW/PopUpSD
        delay: 10
```

The above macro instruction has three commands. The second command occurs 2 seconds aftre the first one, and only if `AirbusFBW/PopUpStateArray[7]` is zero (i.e. not set). The third command call the same popup display to make it dissappear after 10 seconds.
