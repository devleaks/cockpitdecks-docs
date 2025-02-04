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

# Macro Instruction

A MacroInstruction is a collection of instructions. Each instruction has 
- A condition to satisfy before the instruction is issued
- A delay, in seconds, to wait before the instruction is started; the delay starts when the instruction is run, after the above condition is satified.

The condition is a Formula. The result of the formula is interpreted as False if result is 0, or True otherwise.
