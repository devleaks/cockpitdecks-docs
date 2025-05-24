Activation occurs when a Button is requested to handle an Event.

A first step consists of the Activation preparation and initialization. This occurs when the deck is installed and each page created.

Initialization of the Activation will result in the creation of one or more Instructions. In the process, the  Performer entity responsible for executing the Instruction is clearly identified. For example, CockpitInstruction are performed by the Cockpit entity, while SimulatorInstructions are executed by the simulator software.

The initialization of the Activation result in a global status is_valid() that returns True if the Activation contains all information necessary for handling events.

When the Button receives the event, it calls the activate() function on its activation, supplying the event.

```py
	# In the Button class
	def activate(self, event) -> bool:
		result = self._activation.activate(event)
		if result:
			self.render()
		else:
			logger.warning("there was an issue handling the event")
```

Activate() functions return the status of the execution, True if all instructions were carried out without error.

The Activation activate() function always proceeds in similar patterns:

1. It first checks whether the Activation is capable of handling the event.
2. Depending on the event type, it executes one or more Instructions, collecting the result of the execution.
3. When all Instructions have completed, it returns a global status for the entire Activation.

Here is a pseudo-code that handles button press events:

```py
	# In the Activation class
    def activate(self, event: PushEvent) -> bool:
	    status = False
        if not self.can_handle(event):
            return False
        if not super().activate(event):
            return False
        if event.pressed:
            status = self.instruction.execute()
        return status  # Normal termination
```

From then on, the Button must decide what to do next, including if necessary regenerating its appearance.
