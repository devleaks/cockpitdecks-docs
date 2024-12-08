An Instruction is some piece of code that needs to be carried out.

# CockpitdecksInstruction

A Cockpitdecks instruction is a internal Cockpitdecks instruction performed inside the Cockpit.

The Instruction receives the current Cockpit as a parameter to execute its instruction.

# SimulatorInstruction

A SimulatorInstruction is sent to the Simulator for execution. Examples of SimulatorInstructions are executing of commands, or update of simulator data value.

The Instruction receives the current Simulator as a parameter to execute its instruction.

# ButtonInstruction

A ButtonInstruction is a hook to allow for custom, user defined instruction to be executed.

The Instruction receives the current Button it is associated with as a parameter to execute its instruction. The Button has programmatic access to the Deck it belongs, to the Cockpit, and to the Simulator.
