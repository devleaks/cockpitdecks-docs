Observables are data that is constantly monitored. When the data matches a criteria, a list of actions or commands is executed.

For people familiar with the concept, it can be considered a *if-this-then-that* instruction. For example: If the weather data from the simulator indicates rain is falling on the aircraft, we can set the windshield wipers automatically.

Observables are defined at the aircraft level.

# Definition

The file `observables.yaml` is located in the `deckconfig/resources` folder of an aircraft. It is loaded on startup.

```
observables:
  - name: example
    trigger: onchange
    dataref: some/dataref/to/check
    actions:
      - command: do/some/action
        delay: 5
```

## Attribute

### Observables

The `observables` attribute is a list of individual observables.

# Observable

## Attributes

### Trigger

#### True/False Evaluation

The `trigger` attribute of an observable is a single dataref or a formula that is evaluated each time a dataref mentioned in the formula changes. The result of a the formula is compared to 0, i.e. True (different from zero) or False (equal to zero).

If the result of the formula is True, commands listed into the `actions` attributes are executed.

#### Value Changed

Another method to trigger the flow of action(s) is to select the *value changed* mode. Actions get executed as the value of the dataref or of the computation has changed.

### Observable

The observable is the data that is monitored. It can either be a single dataref or a formula.

### Actions

The `actions` attribute is a list of individual actions that gets carried out in sequence if the trigger attribute is True.

# Action

## Attributes

An action as the same attribute as a command carried out by a button:

- instruction
- delay
- condition
