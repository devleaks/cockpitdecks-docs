A button view attribute is originally a X-Plane command that is submitted after the activation completed, as an additional command. The original intend is to propose a new view after some action to speed up information collection in the cockpit. For example, when the pilot selects APU on the ECAM display, the `view` attribute specifies a command to change the view and focus on the ECAM display.

To achieve this, the view attribute value is a command to execute:

```Yaml
view: x-camera/view/8
```

The command to execute is a regular command, a command block, or a macro-instruction. (See [[Button Activation]].)