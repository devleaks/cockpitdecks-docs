A button view attribute is originally a X-Plane command that is submitted after the activation completed, as an additional command. The original intend is to propose a new view after some action to speed up information collection in the cockpit. For example, when the pilot selects APU on the ECAM display, the `view` attribute specifies a command to change the view and focus on the ECAM display.

To achieve this, the view attribute value is a (single) X-Plane command to execute:

```Yaml
view: x-camera/view/8
```

This document explains a new expression for the view attribute value to allow for the execution of one or more commands.

If this schema is successful, it will be extended to other definitions that use a *command* attribute.

# Syntax

```yaml
view:
  - command: command/to/execute
    delay: 2
  - set-dataref: dataref/to/set
    formula: ${dataref/value} 2 +
```

The new view attribute value is a list of command blocks.

Each command block is a command, an action that will be executed, and a few additional optional parameters.

## Command Block

A command block contains a command which can be:

- An X-Plane command,
- An instruction to write a value in a dataref.

In the latter case, the dataref being modified can either be a regular X-Plane dataref, or a Cockpitdecks internal dataref.

### Command

```yaml
command: command/to/execute
```

### Set Dataref

```yaml
set-dateref: dataref/to/set
```

If the set dataref does not have any other attribute, the value will be set to the value of the button.

However, it is possible to specify an alternate value to write:

```yaml
set-dateref: dataref/to/set
formula: ${dataref/value} 2 +
```

To write another value than the button value, the `set-dataref` attribute must be accompanied by a `formula` attribute. The value resulting from the formula computation will be written to the dataref.

### Options

The following attributes can complement to above command.

#### Delay

If specified, a delay is added after the execution of the last command and before the execution of this command.

```yaml
command: command/to/execute
delay: 5
```

Command above will be executed 5 seconds after the execution of the activation or 5 seconds after the execution of the previous command.

#### Condition

A `condition` attribute is a formula that is executed before he command. If the result of the formula is evaluated to a non null (non zero) value, the command is executed. Otherwise, the command is not executed.
# Processing

Detection of the new parameter value is easy. Either it is a string representing a command to submit to X-Plane, or it is a list of one or more command blocks. There is no ambiguity.
# Usage

The `view` attribute can be used as a test bed for execution of more than one command in a sequence, a kind of execution of « macro » command, with additional features like conditions and delay of execution.

Example of use:

When executing a command to trigger a status view, you can imagine the following sequence:

1. Regular button: perform the action.
2. View: move the camera to control the effect of the action.
3. Wait a few seconds… (delay).
4. Restore the original view *if there was no alarm* (this is a condition of execution).

Other sequences can of course be imagined. Literally, the sky is the limit.

This new view attribute value and its development paves the way of a new, more complex, more flexible action definition.
